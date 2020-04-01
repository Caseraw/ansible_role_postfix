# Ansible role postfix

Managing postfix.

[![Build Status](https://travis-ci.org/Caseraw/ansible_role_postfix.svg?branch=master)](https://travis-ci.org/Caseraw/ansible_role_postfix) [<img src="https://img.shields.io/ansible/role/46849">](https://galaxy.ansible.com/caseraw/ansible_role_postfix) [<img src="https://img.shields.io/ansible/role/d/46849">](https://galaxy.ansible.com/caseraw/ansible_role_postfix) [<img src="https://img.shields.io/ansible/quality/46849">](https://galaxy.ansible.com/caseraw/ansible_role_postfix)

- [Ansible role postfix](#ansible-role-postfix)
  - [License](#license)
  - [Author Information](#author-information)
  - [Requirements](#requirements)
  - [Dependencies](#dependencies)
  - [Compatibility](#compatibility)
  - [Role Variables](#role-variables)
  - [Example Playbook](#example-playbook)
  - [Useful shell commands](#useful-shell-commands)
  - [Additional documentation resources](#additional-documentation-resources)
  - [Testing with Molecule](#testing-with-molecule)
  - [CI/CD with Travis CI](#cicd-with-travis-ci)
  - [Useful links](#useful-links)

## License

MIT / BSD

## Author Information

- Made and maintained by: [Kasra Amirsarvari](https://www.linkedin.com/in/caseraw)
- Ansible Galaxy community author: <https://galaxy.ansible.com/caseraw>
- Dockerhub community user: <https://hub.docker.com/u/caseraw>

## Requirements

- Ensure a package manager is available and configured with the correct package sources and repositories.
- Ensure privileged permissions are set for the user executing this role to:
  - Install packages.
  - Edit postfix configuration files.
  - Run postfix/mail specific commands.

## Dependencies

N/A

## Compatibility

Compatible with the following list of operating systems:

- CentOS 7
- CentOS 8
- RHEL 7.x
- RHEL 8.x

## Role Variables

| Variable name | Description |
|---------------|-------------|
| role_postfix_required_packages | List of required packages. |
| role_postfix_remote_path_main_cf | Postfix main.cf configuration file path. |
| role_postfix_main_dot_cf_parameters | Postfix main.cf configuration parameters. |

## Example Playbook

```yaml
---
- name: Manage postfix
  become: True
  gather_facts: False
  tasks:
    - import_role:
        name: ansible_role_postfix
      vars:
      role_postfix_main_dot_cf_parameters:
        - 'inet_interfaces = loopback-only'
        - 'inet_protocols = all'
        - 'mydomain = {{ ansible_domain }}'
        - 'myhostname = {{ ansible_fqdn }}'
        - 'mynetworks = 127.0.0.0/8'
        - 'myorigin = $mydomain'
        - 'mydestination = '
        - 'relayhost = [relayhost.example.com]'
        - 'local_transport = error: local delivery disabled'
        - 'smtp_host_lookup = dns, native'
        - 'smtp_tls_security_level = may'
        - 'smtpd_banner = $myhostname ESMTP'
        - 'debugger_command = PATH=/bin:/usr/bin:/usr/local/bin:/usr/X11R6/bin ddd $daemon_directory/$process_name $process_id & sleep 5'

...
```

## Useful shell commands

```shell
postfix check
postfix status
```

## Additional documentation resources

<www.postfix.org/postconf.5.html>

## Testing with Molecule

This role is locally tested with the use of [Molecule](https://molecule.readthedocs.io/en/latest/), the configuration is located at: [molecule/default](molecule/default).  
The Molecule tests are run (using the [docker driver](https://molecule.readthedocs.io/en/latest/configuration.html#docker)) on [Dockerhub images](https://hub.docker.com/u/caseraw) built for this purpose:

- [CentOS](https://hub.docker.com/r/caseraw/ansible-molecule-centos)
- [Fedora](https://hub.docker.com/r/caseraw/ansible-molecule-fedora)

Some specific configurations might require a full OS instead of a minimal container image. In these use-cases make use of [molecule driver for vagrant](https://molecule.readthedocs.io/en/latest/configuration.html#vagrant) with the [libvirt provider](https://molecule.readthedocs.io/en/latest/configuration.html#molecule-vagrant-module). The Molecule driver and platform configuration part could look something like this:

```yaml
driver:
  name: vagrant
  provider:
    name: libvirt
platforms:
  - name: ansible_role_postfix-ansible-molecule-centos-7
    box: centos/7
    imemory: 1024
    cpus: 1
```

## CI/CD with Travis CI

This role uses [Travis CI](https://travis-ci.org/) to run online tests with the use of [Molecule](https://molecule.readthedocs.io/en/latest/) and pushes notifications to import the role into [Ansible Galaxy](https://galaxy.ansible.com/) once the tests are successful. The Travis CI configuration is located at the root of the Ansible role [.travis.yml](.travis.yml)

## Useful links

- GitHub repository: <https://github.com/Caseraw/ansible_role_postfix>
- Travis CI build status: <https://travis-ci.org/Caseraw/ansible_role_postfix>
- Ansible Galaxy role: <https://galaxy.ansible.com/caseraw/ansible_role_postfix>
