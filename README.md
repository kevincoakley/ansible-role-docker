ansible-role-docker
===================

[![Build Status](https://travis-ci.org/kevincoakley/ansible-role-docker.svg?branch=master)](https://travis-ci.org/kevincoakley/ansible-role-docker)

Manage Docker conatiners natively or using systemd. 

Requirements
------------

Tested with CentOS 7, Ubuntu 16.04 and Project Atomic. Docker needs to be installed.

Role Variables
--------------

    container_state: started

See Ansible Docker module documentation at [http://docs.ansible.com/ansible/docker_module.html](http://docs.ansible.com/ansible/docker_module.html).

    container_privileged: false

Run the container with privileged permissions.

    container_run_as_service: false

If false then the role will run the container natively using the docker command. If true then the role will create a unit file in /etc/systemd/system/ and run the service from systemd. 

    container_pause_seconds: 1

Time to pause after starting the container in case the container needs a few seconds to get up and running.

Dependencies
------------

None

Example Playbook
----------------

    - name: Create two docker containers
      hosts: docker-native
    
      vars:
        - container_state: reloaded
        - container_privileged: true
        - docker_containers:
            - name: ubuntu-container
              image: kevincoakley/ubuntu16.04-systemd
              expose:
                - "8080"
              ports:
                - "2200:22"
                - "8081:8080"
              volumes:
                - "/sys/fs/cgroup:/sys/fs/cgroup"
            - name: centos-container
              image: kevincoakley/centos7-systemd
              expose:
                - "8080"
              ports:
                - "2222:22"
                - "8082:8080"
              volumes:
                - "/sys/fs/cgroup:/sys/fs/cgroup"
    
      roles:
        - ansible-role-docker
    
      tags:
        - docker-start
    
    
    - name: Create two docker containers using systemd
      hosts: docker-systemd
    
      vars:
        - docker_mocker: true
        - container_run_as_service: true
        - container_privileged: true
        - docker_containers:
            - name: ubuntu-container
              image: kevincoakley/ubuntu16.04-systemd
              expose:
                - "8080"
              ports:
                - "2200:22"
                - "8081:8080"
              volumes:
                - "/sys/fs/cgroup:/sys/fs/cgroup"
              ulimits:
                - "nofile=40000:40000"
            - name: centos-container
              image: kevincoakley/centos7-systemd
              expose:
                - "8080"
              ports:
                - "2222:22"
                - "8082:8080"
              volumes:
                - "/sys/fs/cgroup:/sys/fs/cgroup"
    
      roles:
        - ansible-role-docker
    
      tags:
        - docker-systemd
        
License
-------

BSD

Author Information
------------------

Kevin Coakley (https://github.com/kevincoakley)
