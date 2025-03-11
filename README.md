# Ansible Project: Static Web Server Deployment on Amazon Linux................

Project Overview:
This project demonstrates automated deployment of a static web server using Ansible on Amazon Linux 2 servers. The Ansible control node manages two target nodes, installing Nginx and deploying a simple static website.

Architecture:
Server 1: Ansible Control Node
Server 2: Web Server 1 (Managed Node)
Server 3: Web Server 2 (Managed Node)

Prerequisites
Three Amazon Linux 2 EC2 Instances
SSH Access to all servers (key pairs configured)
ansible installed on Server 1 (Control Node)

Project Setup Steps
1. Provision EC2 Instances
Launch three Amazon Linux 2 instances on AWS:

Server 1: ansible-node
Server 2: webserver1
Server 3: webserver2

Setup:

Configure Ansible Control Node (Server 1)
sudo yum update -y
sudo amazon-linux-extras enable ansible2
sudo yum install ansible -y

Verify Ansible Installation:
ansible --version

Configure SSH Access:
During ec2 instance deployment, create keys and make it same for all servers.
Then copy the ssh keys to all servers .

Test password less ssh connection:
ssh ec2-user@<webserver1-ip>
ssh ec2-user@<webserver2-ip>


 Configure Ansible Inventory:

 #vim /etc/ansible/hosts

[webservers]
webserver1 ansible_host=<webserver1-ip> ansible_user=ec2-user
webserver2 ansible_host=<webserver2-ip> ansible_user=ec2-user


Ansible Playbook: Install and Deploy Nginx
Create a directory:
mkdir -p playbook
Playbook File (playbooks/nginx_setup.yml)
---
- name: Setup Nginx Web Servers
  hosts: webservers
  become: yes
  tasks:
  
    - name: Update all packages to the latest
      yum:
        name: '*'
        state: latest
    
    - name: Install Nginx
      yum:
        name: nginx
        state: present
    
    - name: Start and enable Nginx service
      service:
        name: nginx
        state: started
        enabled: yes
    
    - name: Deploy static index.html
      copy:
        src: ../files/index.html
        dest: /usr/share/nginx/html/index.html
        owner: nginx
        group: nginx
        mode: '0644'


Create Static Web Page: index.html

Run the playbook:

#ansible-playbook nginx_setup.yml

Done!

check both server by typing of their Ip!




