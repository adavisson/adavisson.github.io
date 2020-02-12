---
layout: post
title:      "Automating with Ansible"
date:       2020-02-12 23:09:19 +0000
permalink:  automating_with_ansible
---


One tool that I learned to use in my past life as a Systems Administrator that has made my life so much easier is Ansible. I used it in the past to help manage a Linux HPC cluster, but I have recently implemented it at my home to manage my media server and web server. Note: This blog post is not intended to be a 'how to' for Ansible, but instead it is intended to highlight some of the features that make it useful and easy to use.

For those who are unfamiliar, Ansible is an automation tool that, unlike Chef and Puppet, is agentless and push-based. All Ansible needs to run is a password or SSH key to the remote server and the remote server also needs to have Python installed. You contol the remote machines by running 'playbooks' which are just collections of tasks in the form of YAML templates. Let's first take a look at a simple playbook to install available updates on a system using apt package manager: 

```
- name: update server
  apt: 
    update_cache: yes
  become: yes

- name: upgrade server
  apt: 
    upgrade: dist
  notify: Reboot Server
  become: yes

```
The 'name' parameter allows you to declare what is going on so that the output is descriptive of what is going on. Below that is the module that we are using (in this case: apt). Each module has different parameters that you can specify to tell Ansible what you want to do. In the first task we specified the parameter of 'update_cache: yes' which, when looking at the documentation, it says is the equivalent of running 'apt update' on the system. In the next task, we specifiy the parameter of 'upgrade: dist'. This according to the documentation is the equivalent of running 'apt dist-upgrade'. 

A couple other things to note here, both tasks have the parameter 'become: yes'. This is telling Ansible that it needs to use elevated priveleges to run this task. You can specify the password to use to elevate priveleges when you run the playbook with the '--ask-become-pass' flag. Also notice the parameter 'notify: Reboot Server'. This is telling Ansible to run a handler called 'Reboot Server' if that task results in a change. So, if this task upgrades packages then the handler will run, but if the task does not upgrade any packages then the handler will not run. The handler 'Reboot Server' looks like this:

```
handlers:
  - name: Reboot Server
    reboot:
    become: true
```
Okay, now that we know the basics of a playbook how do we target specific hosts? We create an inventory file that specifies all of our hosts and groups them based on function. An inventory file is just a basic text file and would look similar to this:

```
[webservers]
webserver01.example.com
webserver02.example.com
webserver03.example.com

[dbservers]
db01.example.com
db02.example.com
```

Here, we have 5 servers that we have divided into two groups: 'webservers' and 'dbservers'. To get our playbook to target the correct hosts we specify it with the hosts parameter at the top of our playbook like this:

```
- hosts: webservers
```

This would target all of the servers that are a part of the 'webservers' group and no others. We could also target an individual server if we like, but it would be good practice to go ahead and create a group even if you have one server so that if you add more servers in the future you are only updating the inventory file and not the playbook as well.

Ansible can take a little bit of time to learn how to get it set up and organized properly, but once you do it is incredibly flexible and useful. There are also a ton of different modules out there for you to implement and use. More on the basics can be found here: https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html
