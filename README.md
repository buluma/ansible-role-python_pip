# Ansible role [python_pip](https://galaxy.ansible.com/ui/standalone/roles/buluma/python_pip/documentation)

Install pythons pip on your system.

|GitHub|Version|Issues|Pull Requests|Downloads|
|------|-------|------|-------------|---------|
|[![github](https://github.com/buluma/ansible-role-python_pip/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-python_pip/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-python_pip.svg)](https://github.com/buluma/ansible-role-python_pip/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-python_pip.svg)](https://github.com/buluma/ansible-role-python_pip/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-python_pip.svg)](https://github.com/buluma/ansible-role-python_pip/pulls/)|[![Ansible Role](https://img.shields.io/ansible/role/d/buluma/python_pip)](https://galaxy.ansible.com/ui/standalone/roles/buluma/python_pip/documentation)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-python_pip/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true
  gather_facts: true

  pre_tasks:
    - name: Update apt cache.
      apt: update_cache=yes cache_valid_time=600
      when: ansible_os_family == 'Debian'
      changed_when: false

    - name: Check if python3.11 EXTERNALLY-MANAGED file exists
      ansible.builtin.stat:
        path: /usr/lib/python3.11/EXTERNALLY-MANAGED
      register: externally_managed_file_py311

    - name: Rename python3.11 EXTERNALLY-MANAGED file if it exists
      ansible.builtin.command:
        cmd: mv /usr/lib/python3.11/EXTERNALLY-MANAGED /usr/lib/python3.11/EXTERNALLY-MANAGED.old
      when: externally_managed_file_py311.stat.exists
      args:
        creates: /usr/lib/python3.11/EXTERNALLY-MANAGED.old

    - name: Check if python3.12 EXTERNALLY-MANAGED file exists
      ansible.builtin.stat:
        path: /usr/lib/python3.12/EXTERNALLY-MANAGED
      register: externally_managed_file_py312

    - name: Rename python3.12 EXTERNALLY-MANAGED file if it exists
      ansible.builtin.command:
        cmd: mv /usr/lib/python3.12/EXTERNALLY-MANAGED /usr/lib/python3.12/EXTERNALLY-MANAGED.old
      when: externally_managed_file_py312.stat.exists
      args:
        creates: /usr/lib/python3.12/EXTERNALLY-MANAGED.old

  roles:
    - role: buluma.python_pip
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-python_pip/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: true
  gather_facts: false

  roles:
    - role: buluma.bootstrap
    - role: buluma.epel
    - role: buluma.buildtools
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-python_pip/blob/master/defaults/main.yml):

```yaml
---
# defaults file for python_pip

# By default no modules should be installed. Note: This does not work on Debian Bookworm and Ubuntu noble.
# See https://peps.python.org/pep-0668/
python_pip_modules: []

# Connect to a (pypi) proxy by setting this variable.
# python_pip_proxy: "https://user:password@proxy:8443/artifactory/pypi/pypi-virtual/simple"

# Don't forget to trust foreign pip repositories if you use them.
# python_pip_trusted_host: my-pip-repository.example.com

# You can have this role update pip, using pip.
python_pip_update: true

# You can use something other than the default pip binary.
# python_pip_executable: pip3
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-python_pip/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | Version |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Ansible Molecule](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-bootstrap.svg)](https://github.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.buildtools](https://galaxy.ansible.com/buluma/buildtools)|[![Ansible Molecule](https://github.com/buluma/ansible-role-buildtools/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-buildtools/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-buildtools.svg)](https://github.com/shadowwalker/ansible-role-buildtools)|
|[buluma.epel](https://galaxy.ansible.com/buluma/epel)|[![Ansible Molecule](https://github.com/buluma/ansible-role-epel/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-epel/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-epel.svg)](https://github.com/shadowwalker/ansible-role-epel)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-python_pip/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[Alpine](https://hub.docker.com/r/buluma/alpine)|all|
|[Debian](https://hub.docker.com/r/buluma/debian)|all|
|[EL](https://hub.docker.com/r/buluma/enterpriselinux)|9, 8|
|[Fedora](https://hub.docker.com/r/buluma/fedora)|39, 38, 40|
|[opensuse](https://hub.docker.com/r/buluma/opensuse)|all|
|[Ubuntu](https://hub.docker.com/r/buluma/ubuntu)|focal, bionic, jammy, lunar, noble|

The minimum version of Ansible required is 2.12, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-python_pip/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-python_pip/blob/master/CHANGELOG.md)

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-python_pip/blob/master/LICENSE)

## [Author Information](#author-information)

[Shadow Walker](https://buluma.github.io/)
