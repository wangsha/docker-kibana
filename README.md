docker-kibana
============

[![Build Status](https://travis-ci.org/wangsha/docker-kibana.svg?branch=master)](https://travis-ci.org/wangsha/docker-kibana)
[![Ansible Galaxy](https://img.shields.io/badge/AnsibleGalaxy-wangsha.docker--kibana-blue.svg)](https://galaxy.ansible.com/wangsha/docker-kibana/)

Ansible role to manage and run the kibana docker container.

Requirements
------------

This role has only been tested on Ubuntu 14.04. Since this uses Ansible's
docker module, you will need to ensure that a recent-ish version of `docker-py`
and `docker` are installed.

Examples
--------

Install this module from Ansible Galaxy into the './roles' directory:
```bash
ansible-galaxy install wangsha.docker-kibana -p ./roles
```

Use it in a playbook as follows, assuming you already have docker and elasticsearch setup:
```yaml
- hosts: 'servers'
  roles:
    - { role: 'wangsha.docker-kibana', elasticsearch_url: "http://localhost:9200"}
```

```yaml
---
- name: Set up elasticsearch and kibana
  hosts: all
  vars:
    docker_kibana_ports:
      - 80:5601
  roles:
    - role: 'wangsha.docker-elasticsearch'
    - { role: 'wangsha.docker-kibana', elasticsearch_url: "http://localhost:9200"}
```

Have a look at the [defaults/main.yml](defaults/main.yml) for role variables
that can be overridden.

If you need a playbook to set Docker itself, have a look at [angstwad.docker_ubuntu](https://github.com/angstwad/docker.ubuntu) Galaxy role.

Default docker image used is [kibana/kibana](https://hub.docker.com/r/kibana/kibana/). Default port is 5601.


License
-------

[MIT](LICENSE.txt)

Author Information
------------------

- wangsha
