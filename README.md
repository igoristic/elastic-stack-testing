# elastic-stack-testing

Automated Integration Testing (AIT)

## Infrastructure

 - Software products under test: Elasticsearch, Kibana, Logstash, Beats
 - Ansible is used to install and configure the software products under test
 - Python, Pytest and Selenium will be used for the test framework
 - Automated virtual machine support for Vagrant boxes and AWS EC2 instances

## Directory Structure

```
elastic-stack-testing/
  ansible/         directory for Ansible
  buildenv.sh      shell script for running scripts locally
  playbooks/       Ansible playbooks
  tests/           test scripts
  vm/              automated VM files
```

## Environment Setup

 * Install Virtualbox and Vagrant

  - https://www.virtualbox.org/wiki/Downloads
  - https://www.vagrantup.com/downloads.html

* Install Python 3

  In this repo see file: .python-version for version to install

  https://www.python.org/downloads/

* Install Ansible

  In this repo see file: .ansible-version for version to install (It is recommended to use pip install ansible==version)

  http://docs.ansible.com/ansible/latest/intro_installation.html

## Runnning Scripts

* buildenv.sh script will start the VM and run the playbook

  Fill in information below and then run ./buildenv.sh   

  - Build Variables
    - ES_BUILD_URL - build URL format: server/build_num-hash   
    - ES_BUILD_PKG_EXT - package extension one of: tar, rpm, deb

    - Ansible Standalone Variables (Product installation only - no Pytest) [** Phase 1 Pilot Testing ** ]
      - AIT_ANSIBLE_PLAYBOOK - playbook for product installation   
        Example: AIT_ANSIBLE_PLAYBOOK=${AIT_ANSIBLE_PLAYBOOK_DIR}/install_no_xpack.yml

      - AIT_ANSIBLE_SCRIPT - machine setup which calls above playbook      
        Example: AIT_ANSIBLE_SCRIPT=${AIT_SCRIPTS}/shell/setup_vagrant_vm.sh   

* To run playbook on already running VM:
    - source aitenv.sh
    - export WORKSPACE=${AIT_ROOTDIR}/ait_workspace
    - ANSIBLE_GROUP_VARS=/${WORKSPACE}/vars.yml AIT_UUT=${AIT_UUT} ansible-playbook ${AIT_ANSIBLE_PLAYBOOK}.yml


* Pytest Variables [** Phases 2/3 Pilot Testing ** ] - coming soon

## Pilot Testing

  - Phase 1: Ansible standalone product installation/configuration
      - Machine: vagrant/virtualbox
      - Machine OS: ubuntu-16.04-x86_64
      - Node: single
      - Software Versions: 5.6.x and 6.0.0
      - Package: tar.gz
      - X-pack: with and without  


  - Phase 2: Pytest tests - coming soon

  - Phase 3: Integrated Pytest ansible/tests - coming soon

## Using Vagrant CLI

  If using Vagrant/Virtualbox through buildenv.sh script, a directory is created in ait_workspace
  that contains the Vagrantfile.  The directory is prefixed with the build id.  

  If the VM is running and you want to ssh or destroy the vagrant machine, the AIT_ANSIBLE_PLAYBOOK env must
  be set but does not have to point to an actual playbook.

  AIT_ANSIBLE_PLAYBOOK=1 vagrant ssh
  AIT_ANSIBLE_PLAYBOOK=1 vagrant destroy -f

  If doing a vagrant up, then AIT_ANSIBLE_PLAYBOOK must point to a valid playbook.

## Contact

  For help please contact: liza.dayoub@elastic.co
