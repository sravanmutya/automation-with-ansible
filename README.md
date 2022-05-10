# automation-with-ansible

The goal of this assignment is to implement a simple app in python-flask and put it behind HAproxy - which will work as a load balancer. 

The basic service is a simple (flask) app, that only answers requests, by replying with the time and hostname of the host that replied. The service is found in the following git repository: https://github.com/patrikarlos/NSO_A2 (Links to an external site.). Use the 'application2.py' for this assignment. 

Three nginx servers A, B, C are running on three different platforms, and all three have different addresses.

A Bastion is used as an entry point to the network of the three servers, it is basically a jump host to the the network. 

The communication to the servers through the Bastion is secured using SSH. 

The Bastion is used for secure access to the network for configuration and administration.

A server HAproxy is used to load balance between the three servers. The HAproxy is the entry point for a user to the website. 

PHP script is deployed on Nginx webservers and HAproxy using Ansible.

This project need to have a SSH config that can be used by Ansible.

