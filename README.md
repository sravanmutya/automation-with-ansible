# automation-with-ansible



The statement of the problem -

The goal of this assignment is to implement a simple app in python-flask and put it behind HAproxy - which will work as a load balancer. 

The basic service is a simple (flask) app, that only answers requests, by replying with the time and hostname of the host that replied. The service is found in the following git repository: https://github.com/patrikarlos/NSO_A2  The 'application2.py' is used for this assignment. 

Three servers A, B, C are running on three different platforms, in a private network, in which all three have different IP addresses in the range, 10.0.1.0/27. Specifically, for this assignment, the IP addresses of the servers are as follows -


A Bastion is used as an entry point to the network of the three servers, it is basically a jump host to the the network. 

The communication to the servers through the Bastion is secured using SSH. 

The Bastion is used for secure access to the network for configuration and administration.

A server HAproxy is used to load balance between the three servers. The HAproxy is the entry point for a user to the website. 

This project need to have a SSH config that can be used by Ansible.



Background research -

Ansible is an automation engine that has the following features -
1. Configuration management of computing devices in an industrial network, etc.
2. Deployments of servers, computing devices etc. in industrial setting, network etc.
3. Provisioning of packages, software, etc.
4. Security - ansible is configured through SSH - Secure Shell, hence communication via ansible is secured, as the devices are remotely accessed through a host running ansible.

Inventory files -
Ansible has inventory files that contain information about the remote devices to be configured/managed through ansible playbooks. They could contain the ip addresses or the domain names of the remote devices.

Example ansible command -

ansible all -m ping    # This command pings all the servers/devices in the inventory

Working format of ansible -
1. Each specific task in ansible is written through a module.
2. Multiple modules are written in sequential order.
3. Multiple modules for related tasks is called a play.
4. All plays together makes a playbook.
5. Playbook is written as a file format called YAML.

Understanding of the format -
     | --> Install httpd                 --> Module -->} Play }
Task-| --> Enable httpd                  --> Module -->}      } Playbook
     | --> Start httpd                   --> Module -->| Play }
     | --> Enable http port on firewall  --> Module -->|      }


# References - 
1. Notes and material provided by Complete DevOps Ansible Automation Training course by Imran Afzal on Udemy. ( https://www.udemy.com/share/106iDy3@Wnf7pv-V0J6Q5RjdnO_a6ue0di-S0DQpkbrq0OfaBMJ63Ji8vSbB-yqMGSTB8G_-/ )
2. Google search for keywords that were not understood such as Git, yaml, ansible etc.



# Appendix
Assignment - 2 workshop notes -

1. Setting up the environment - servers, SSH
2. Programming

-> Set up Ubuntu server
-> Install nginx, php, php-fpm packages.

Ubuntu 20.04 LTS - use CityNetwork/Cleura VMs (or) use other VMs from Azure, AWS, etc.

                |   PHP  |
                     |
                     |
                |  Nginx |
                     |
                     |
                | Ubuntu |
                 
                 
From the local machine SSH to Bastion host.

There will be five VMs on CityNetwork/ Cleura/ AWS/ Azure/ other, atleast one more on your local machine to access.

The Bastion is used as an entry point to the cloud network, it is not possible to get to the other VMs without going through the Bastion. 

The local machine is outside the internal network, i.e., you connect through the Bastion host for security purposes.

Three nginx servers running on three different platforms.

Servers A, B, C --> serve the same purpose.

Three different servers with three different addresses.

Better to make the three on the same network, i.e. -
-> The underlying network
-> IP address - subnetting

There are two public addresses needed on the cloud network -
-> One for the bastion host
-> One for the HAProxy load balancer

HAProxy and Bastion will have two interfaces one for private IP and one for public IP, i.e., Internet.

-> So 5 private addresses are needed.

Throughout SSH is used for encryption of the communication.
-> Particularly to send shell commands to another host in a secure way.

Bastion is in between the local machine and the internal network. Hence, it should be as secure as possible.

-> Bastion is also jump host, i.e, you need to first get into the Bastion and from there into the servers A, B, and C.

However, the internal servers A, B, and C never need to log into the Bastion host.

Therefore A, B and C keys need to be present in the Bastion. But the Bastion key doesn't need to be in A, B and C.

-> ssh config file
e.g. ssh bob (from Alice)

-> Purpose of the ssh config file is that it simplifies the usage of SSH instead of using the entire command IP address you can use the name assigned to the host. For example, ssh Bastion

SSH -> symmetrical encryption, that means,
-> Both the sides have keys.
-> The session is temporary - communication is established only for a session.

                          |   Local machine   |
                                    |
                                    |
                                    | 
                        |        Internet       |
                             |               |
                -------------|               |----------------
               |             |               |                |
               |        | HAProxy |      | Bastion |          |
               |      ____________|___________                |------> Cloud network ( City cloud / cleura / AWS / Azure etc.)
               |      |           |          |                |   
               |   Server A    Server B      Server C         |
               |______________________________________________|
              
Through HAProxy anyone can connect to the website.
Through Bastion - secure access for configuration and administration.

There also needs to be a configuratio file for HAProxy for load balancing.

Ansible can be used to configure A, B, C, and HAproxy.

Testing the entire thing -
-> Query the PHP from the webserver.

Testing the loadbalancing -
-> You have to get the same number of replies from all the three servers A, B, and C.

Ansible can be used on WSL - Windows Subsytem for Linux. On Windows 11, now Linux GUI is also there.

All SSH configurations should be correct.

git -> a software for tracking changes in any set of files, usually used for coordinating work among programmers collaboratively developing source code during software development. ( https://en.wikipedia.org/wiki/Git )

site.yaml -> the playbook itself

README -> an explanation - this file

- listen 80 default_server;
- listen [::] :80 default_server; - Any kind of address for IPv6 - [::]

- root /var/www/html;
- index index.php index.html indenx.niginx-debian.html

Python - django

The packages nginx, php have to be installed on the servers A, B, and C.

# YAML file syntax - 
-> No tabs allowed, only spaces.
-> Empty lines have no values.
-> No difference in double or single quotes.
-> Indentation is extremely important.

e.g. 

---
   - name: sample playbook      # This is the name of the playbook
     hosts: all (or) localhost  # Where to run
     become: yes                # Run as a different user
     become_user: root
     
     tasks:
     - name: install apached httpd
       yum:                     # Run task module yum
       name: httpd              # Name of the package
       state: present           # What to do? Install
       
     - name: 2nd task
       service: 
       name: httpd
       state: started
       
 Running ansible without a playbook ( command )
-> ansible 

Running ansible with a playbook (command )
-> ansible-playbook

# Load Balancing: HAProxy

-> frontend: listens for connections
-> backend: forwards requests
-> stats: monitoring load balancer and other two nodes



                
