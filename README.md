# Ansible role: labocbz.install_haproxy

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Cbz-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: Haproxy](https://img.shields.io/badge/Tech-Haproxy-orange)

An Ansible role to install and configure HAProxy on your host.

The Ansible role installs HAProxy, a powerful load balancer, on the target system. It provides a seamless installation process using packages, ensuring a reliable and consistent setup. While the role offers flexibility to configure custom paths for installation, it is advisable to utilize the default paths for a smoother experience.

Along with installation, the role allows administrators to fine-tune the HAProxy configuration by customizing various aspects such as timeout values, log settings, and different options. The role also facilitates the setup of specific error pages for distinct HTTP status codes, enhancing the user experience during error scenarios.

One notable feature of the role is its ability to handle the removal of old and outdated HAProxy configurations. When deploying HAProxy configurations that might change over time, previous configurations may become obsolete. To maintain a clean and up-to-date setup, the role effectively manages the removal of these old configurations, ensuring the system operates with the latest and most relevant settings.

Additionally, the role provides an option to activate the web interface, which allows administrators to visualize load balancer configurations for better monitoring and management. For added security, the role supports SSL/TLS for this interface. Administrators can specify a domain that will be used to generate the PEM file containing the required keys and certificates, ensuring secure access to the web interface.

The role's flexibility empowers administrators to tailor HAProxy according to their specific requirements, making it a versatile solution for load balancing in diverse application environments. With the role's capability to handle old configurations and provide a customizable setup, administrators can confidently deploy a robust and high-performance load balancer tailored to their needs.

In summary, the HAProxy role streamlines the installation and configuration of the load balancer, while also offering features such as managing old configurations, customizable settings, error page handling, and SSL/TLS support for the web interface. By leveraging this role, administrators can deploy a reliable and efficient HAProxy setup, ensuring smooth traffic distribution and enhanced application performance.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
install_haproxy__path: "/etc/haproxy"
install_haproxy__confs_path: "{{ install_haproxy__path }}/conf.d"
install_haproxy__error_path: "{{ install_haproxy__path }}/errors"
install_haproxy__ssl_path: "{{ install_haproxy__path }}/ssl"

install_install_haproxy__group: "haproxy"
install_install_haproxy__user: "haproxy"

install_haproxy__error_files:
  - "400"
  - "403"
  - "408"
  - "500"
  - "502"
  - "503"
  - "504"

install_haproxy__log: "global"
install_haproxy__options:
  - "httplog"
  - "dontlognull"

install_haproxy__timeout_connect: 5000
install_haproxy__timeout_client:  50000
install_haproxy__timeout_server:  50000
install_haproxy__remove_old_confs: true

install_haproxy__listen_stats: true
install_haproxy__listen_stats_https: true
install_haproxy__listen_stats_cert: "{{ install_haproxy__ssl_path }}/my-haproxy-server.domain.tld/my-haproxy-server.domain.tld.pem.crt"
install_haproxy__listen_stats_key: "{{ install_haproxy__ssl_path }}/my-haproxy-server.domain.tld/my-haproxy-server.domain.tld.pem.key"
install_haproxy__listen_stats_fullchain: "{{ install_haproxy__ssl_path }}/my-haproxy-server.domain.tld/fullchain.pem"
install_haproxy__listen_stats_mode: "http"
install_haproxy__listen_stats_bind: "*"
install_haproxy__listen_stats_port: 8181
install_haproxy__listen_stats_uri: "/haproxy/stats"
install_haproxy__listen_stats_refresh: 10
install_haproxy__stats_login: "joe"
install_haproxy__stats_password: "passwd"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_install_haproxy__path: "/etc/haproxy"
inv_install_haproxy__confs_path: "{{ inv_install_haproxy__path }}/conf.d"
inv_install_haproxy__error_path: "{{ inv_install_haproxy__path }}/errors"
inv_install_haproxy__ssl_path: "{{ inv_install_haproxy__path }}/ssl"

inv_install_install_haproxy__group: "haproxy"
inv_install_install_haproxy__user: "haproxy"

inv_install_haproxy__error_files:
  - "400"
  - "403"
  - "408"
  - "500"
  - "502"
  - "503"
  - "504"

inv_install_haproxy__listen_stats: true
inv_install_haproxy__listen_stats_https: true
inv_install_haproxy__listen_stats_cert: "{{ inv_install_haproxy__ssl_path }}/my-haproxy-server.domain.tld/my-haproxy-server.domain.tld.pem.crt"
inv_install_haproxy__listen_stats_key: "{{ inv_install_haproxy__ssl_path }}/my-haproxy-server.domain.tld/my-haproxy-server.domain.tld.pem.key"
inv_install_haproxy__listen_stats_port: 8181
inv_install_haproxy__listen_stats_uri: "haproxy/stats"
inv_install_haproxy__stats_login: "joe"
inv_install_haproxy__stats_password: "passwd"

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_haproxy"
  tags:
    - "labocbz.install_haproxy"
  vars:
    install_haproxy__path: "{{ inv_install_haproxy__path }}"
    install_haproxy__confs_path: "{{ inv_install_haproxy__confs_path }}"
    install_haproxy__error_path: "{{ inv_install_haproxy__error_path }}"
    install_haproxy__ssl_path: "{{ inv_install_haproxy__ssl_path }}"
    install_haproxy__error_files: "{{ inv_install_haproxy__error_files }}"
    install_haproxy__listen_stats: "{{ inv_install_haproxy__listen_stats }}"
    install_haproxy__listen_stats_port: "{{ inv_install_haproxy__listen_stats_port }}"
    install_haproxy__listen_stats_uri: "{{ inv_install_haproxy__listen_stats_uri }}"
    install_haproxy__stats_login: "{{ inv_install_haproxy__stats_login }}"
    install_haproxy__stats_password: "{{ inv_install_haproxy__stats_password }}"
    install_haproxy__listen_stats_https: "{{ inv_install_haproxy__listen_stats_https }}"
    install_haproxy__listen_stats_cert: "{{ inv_install_haproxy__listen_stats_cert }}"
    install_haproxy__listen_stats_key: "{{ inv_install_haproxy__listen_stats_key }}"
  ansible.builtin.include_role:
    name: "labocbz.install_haproxy"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-04-27: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2023-05-27: Bind / SSL

* Added a test battery to check auth if enabled
* Added a bind address for stats
* Added SSL/TLS handling for stats url

### 2023-04-28: Remove old confs

* Remove all previous conf, ssl and errors if we have to run the install playbook again because: "In general, it is a good practice to only add configurations when they are needed, as this minimizes the chance of errors or conflicts. In your case, it makes sense to install Apache and configure it, but not add any vhosts until they are needed by the app. However, you should also consider how to handle the scenario where you need to reinstall Apache. If you reinstall Apache without cleaning up the old vhosts, you may end up with conflicting configurations, which could cause issues with your app or even your entire system." ChatGPT.

### 2023-05-30: Cryptographic update

* SSL/TLS Materials are not handled by the role
* Certs/CA have to be installed previously/after this role use

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

### 2023-12-14: System users

* Role can now use system users and address groups

### 2024-02-22: New support

* New CI
* Added Sonarqube
* Added support for Debian 12/11
* Added support for Ubuntu 22
* Added random for cluster services

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
