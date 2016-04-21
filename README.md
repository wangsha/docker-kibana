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

Default docker image used is [kibana](https://hub.docker.com/_/kibana/). Default port is 5601.

Known Issue
-----------
As of date March 2016, the docker image will restart itself when runtime memory exhaust container's assigned memory, resulting in a constantly changing pid.
To walk around this [issue](https://github.com/elastic/kibana/issues/5170), set environment variable `NODE_OPTIONS="--max-old-space-size=200"`
to a smaller number than container's memory limit.
```yaml
---
- role: wangsha.docker-kibana
      docker_kibana_image: kibana:4.4.2
      elasticsearch_url: http://localhost:9200
      docker_kibana_env:
        NODE_OPTIONS: "--max-old-space-size=300"
        ELASTICSEARCH_URL: "{{ elasticsearch_url }}"
      docker_kibana_memory_limit: 512MB
```

Custom volume mappings
----------------------
Docker allows mounting a host directory or a host file as [data volume](https://docs.docker.com/engine/userguide/containers/dockervolumes/).
This role mounts host directories to persist container data and host files to configure container behavior.
`docker_kibana_directory_volumes` and `docker_kibana_file_volumes` are the two variables to control volume mappings.
If you wish to customize the mapping, please follow `<host directory>:<container directory>:<mapping mode>` format
 to ensure host directories are correctly created before launching containers.
 
To customize host file mappings, update `docker_kibana_file_volumes`. 
This role will automatically create file parent directories and copy the template 
to host machine. The naming convention for template is `<host_file_name>.<host_file_extension>.j2`.
To copy template from your own ansible diretories, set `docker_kibana_template_path`.

Example Config:
```yaml
docker_kibana_file_volumes:
  - '/opt/myapp/conf/settings.conf:/etc/myapp/conf/settings.conf:ro'
docker_kibana_template_path: /path/to/ansible/project/templates/
# make sure file /path/to/ansible/project/templates//settings.conf.j2 exists. 
```

License
-------

[MIT](LICENSE.txt)

Author Information
------------------

- wangsha
