# automation-with-ansible

The goal of this assignment is to implement a simple web service.

Three nginx servers A, B, C with three different addresses are deployed.

A Bastion is used as an entry point to the network for secure access for configuration and administration using SSH. 

A server HAproxy is used to load balance between these three servers. So anyone can connect to the website through this server. 

PHP script is deployed on Nginx webservers and HAproxy using Ansible.

This project need to have a SSH config that can be used by Ansible.
