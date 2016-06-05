## Continuous Delivery environment with Docker on Ubuntu 14.04


### Bare Ubuntu
 `$ ./bareUbuntu.sh`


### Vagrant###
**Pre-requisites:**
  * [Oracle VM VirtualBox](http://www.virtualbox.org)
  * [Vagrant](http://www.vagrantup.com) v1.8.1
  * [Packer](http://www.packer.io) v0.9
  * [Ansible](http://docs.ansible.com/intro_installation.html#latest-releases-via-apt-ubuntu) v2.0

  * Create VirtualBox vagrant managed box from ubuntu iso image

**Build instructions**
  ```
  $ git clone https://github.com/shiguredo/packer-templates.git (-r 36b5694514efd858a1a7a4e3c90112e8cb8a61ab)
  $ cd packer-templates/ubuntu-14.04
  $ packer build -only=virtualbox-iso template.json
  $ vagrant box add ubuntu-14-04-x64-virtualbox ubuntu-14-04-x64-virtualbox.box
  ```
 * Customize image ([add](ansible/playbook.yml) desired packages: docker, docker-compose, etc)

  ```
  $ vagrant up
  $ vagrant package --output ubuntu-14-04-x64-virtualbox-cd-1.0.2.box
  $ vagrant box add ubuntu-14-04-x64-virtualbox-cd-cd-1.0.2 ubuntu-14-04-x64-virtualbox-cd-cd-1.0.2.box
  ```
 * Run image

  ```
  $ cd demo
  $ vagrant up
  ```