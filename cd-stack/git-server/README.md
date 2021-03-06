# Git server on Docker
</br>

Raw [Git Server](https://git-scm.com/book/en/v1/Git-on-the-Server) (ssh & git protocols)

#### Run docker git-server container
  `$ docker-compose up -d`

#### Add repository to git server
1. Create bare repository, either initiated as an empty repository or cloned from an existing repository.

  ```
  $ git init --bare ionradan.git                        # create empty bare repository
  $ git clone --bare repo_to_bare_clone ionradan.git    # create bare repository from an existing repository
  ```
1. Copy repository to git server

  ```
  $ docker cp ionradan.git git-server:/opt/git/repositories    # move bare repository to git server
  # unfortunately permissions are not right, we need manual intervention
  $ docker exec -it git-server chown -R git:git /opt/git/repositories
  ```
1. Clone repository

 * ssh protocol

    ```
    $ git clone git@localhost:/opt/git/repositories/ionradan.git
    $ git clone ssh://git@localhost:2224/opt/git/repositories/ionradan.git    # non-standard ssh port
    ```

 * git protocol (read only)

    ```
    $ git clone git://localhost/ionradan.git
    ```
1. Backup & Restore

  [Reference](https://docs.docker.com/engine/tutorials/dockervolumes/)

  ```
     $ docker run --rm --volumes-from git-server -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /opt/git/repositories    # backup, will create a file git-repositories.tar
     $ docker run --rm --volumes-from git-server -v $(pwd):/backup ubuntu bash -c "cd /opt && tar xvf /backup/backup.tar --strip 1"    # restore
 ```

##### Add key
 * Add key to `files/authorized_keys` and rebuild image. Compromise to achieve the immutable environment principle.

##### Generate keys
  * `$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

##### Git shell - how to
  * http://planzero.org/blog/2012/10/24/hosting_an_admin-friendly_git_server_with_git-shell
