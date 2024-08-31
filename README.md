# Ansible Role: Docker

[![Role - system](https://github.com/syndr/ansible-role-docker_engine/actions/workflows/role-system.yml/badge.svg)](https://github.com/syndr/ansible-role-docker_engine/actions/workflows/role-system.yml)

## Overview

This Ansible role facilitates the automated setup and management of Docker environments on various Linux distributions. It encompasses the installation, configuration, and management aspects of Docker, ensuring a seamless and secure Docker experience.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
# The Docker authentication credentials to use when pulling images from a registry
# - The key is the registry URL, and the value is a dictionary with 'user' and 'pass' keys
docker_engine_auths: {}
  # quay.io:
  #   user: ""
  #   pass: ""

# Allow the playbook to continue if a Docker authentication fails (true/false)
docker_engine_auths_allow_failed: false

# The Docker daemon options to set in /etc/docker/daemon.json
# - The options are set as key-value pairs in a dictionary
docker_engine_daemon_options:
  docker_daemon_options:
    log-driver: "json-file"
    log-opts:
      max-size: "20m"
      max-file: "2"

# The name of the Docker application package in the system  package manager
#  - Set to 'DEFAULT' to use the default package name for the system, if available
docker_engine_package: DEFAULT

# The version of the Docker application package to install
#  - Both specific version numbers such as "20.10.25-1.amzn2.0.3" and nonspecific
#    version numbers such as "20.10" are supported.
#  - If a nonspecific version number is provided (IE: "20.10"), the latest version of the package
#    that matches the nonspecific version number will be installed.
#     - If an applicable version is already installed (IE: "20.10.25-1.amzn2.0.3"), and package state
#       is set to 'present', the installed version will be left as is.
docker_engine_package_version: "27"

# The state of the Docker application package (present/absent/latest)
#  - present: The package will be installed
#  - absent: The package will be removed
#  - latest: The package will be updated to the latest version
docker_engine_package_state: present

# A list of extra packages to install alongside the Docker package
docker_engine_extra_packages: []

# Allow this role to make changes which may restart the Docker daemon (true/false)
# NOTE: Restarting the docker daemon will cause running containers to be stopped!
docker_engine_allow_restart: true

# Allow this role to downgrade the Docker package if the installed version is newer than the desired version (true/false)
docker_engine_allow_downgrade: false

# Should this role manage the Docker service (true/false)
docker_engine_service_manage: true
# Should the Docker service be enabled at boot
docker_engine_service_enabled: true
# The state of the Docker service (started/stopped)
docker_engine_service_state: started
# Should the Docker service be restarted when the daemon.json file changes
docker_engine_restart_handler_state: restarted

# List of docker network that should be created for workload containers
#  - All networks will at present be created with the 'bridge' driver and 'internal' set to false
docker_engine_networks:
  - main

# List of usernames that should be given access to docker
docker_engine_users: []
```

## Example Playbook

```yaml
- hosts: servers
  roles:
     - { role: docker-setup, docker_service_state: started }
```

## Usage

1. Include this role in your Ansible playbooks.
2. Override the default variables values in your playbook if necessary.
3. Run your playbook.

> [!IMPORTANT]  
> The version of the Docker engine and the docker-cli application are different things. `docker --version` will show the version of the docker-cli application, while `docker version` will show the version of the Docker engine. The `docker_engine_package_version` variable in this role is used to specify the version of the Docker engine to install, not the docker-cli application.

**'latest' versus 'present'**

The `docker_engine_package_state` variable can be set to 'latest' or 'present'. When set to 'latest', the package manager will attempt to install the latest version of the Docker package available that *matches the value* of `docker_engine_package_version`. When set to 'present', the package manager will install the version specified in the `docker_engine_package_version` variable _only_ if no existing version of docker is discovered. To guarantee that a version of docker matching the specified version number is installed, set `docker_engine_package_state` to 'latest'.

## Requirements

- Python 3.8 or higher.
- Ansible 2.16 or higher.
- Collections
    - `community.general`
    - `community.docker`

## Additional Information

This role is designed to be flexible for various configurations and environments. However, the current implementation has only been tested those distributions listed in [meta/main.yml](meta/main.yml). If you encounter issues with other distributions, please file an issue or submit a pull request.


## Contributing

Contributions are welcome. If you want to contribute, please submit a pull request with proposed changes or file an issue to discuss what you would like to see included.
