# Ansible-Config

Basic Ansible configuration to set up a load balancer, two Web servers, and a database server via a control system

## System Structure Setup:

                ─────────
               │Control
               └─────────
     │──────────────│────────────────│
     │              │                │
    ──────      ──────────────      ──────
    │lb01       │App01 - App02      │db01
    └──────     └──────────────     └──────
    LoadBalancer    Webserver       Database

1.  Install Ansible in the Control Unit:

    Prerequisite: Python

    OS: Fresh Ubuntu Installation

            sudo apt-get install software-properties-common
            sudo apt-add-repository ppa:ansible/ansible
            sudo apt-get update
            sudo apt-get install ansible

    OS: Mac

            pip install --user ansible (or)
            brew install ansible

    Verify Installation:

            ansible-playbook --version

2.  Create a Project folder

3.  Create the inventory.txt file (note the groupings)

        [loadbalancer]
        lb01

        [webserver]
        app01
        app02

        [database]
        db01

        [control]
        ebinisa ansible_connection=local

4.  Parse the hosts into Ansible (inside project folder)

        ansible -i inventory.txt --list-hosts all

    Create ansible.cfg file:
    This helps to parse without mentioning the local inventory

        [default]
        inventory = ./inventory.txt

    Verify:

        ansible --list-hosts all

5.  Create Playbooks
    Create main playbooks/hostname.yml:

        ---
        - hosts: all
          tasks:
            - command: hostname

    Create loadbalancer.yml:

        ---
        - hosts: loadbalancer
          become: true
          tasks:
            - name: install nginx
            apt: name=nginx state=present update_cache=yes

    Create database.yml:

        ---
        - hosts: database
          become: true
          tasks:
            - name: install mysql-server
            apt: name=mysql-server state=present update_cache=yes

    Executive the above using the script below:

        ansible-playbook control.yml
        ansible-playbook playbooks/hostname.yml
        ansible-playbook loadbalancer.yml
        ansible-playbook database.yml

## Tasks:

======

Ping All:

    ansible -m ping all

    ansible -m command -a "hostname" all
    ansible -a "hostname" all

The last 2 are same cos command is the default module
