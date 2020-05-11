# Ansible Role: Install VirtualBox

Install [VirtualBox](https://virtualbox.org) for Linux and Windows with Extension Pack.

## Work on

### Ansible Galaxy style

```yaml
  platforms:
    - name: Ubuntu
      versions:
        - focal
        - eoan
        - disco
        - cosmic
        - bionic
        - xenial
    - name: Debian
      version:
        - jessie
        - stretch
        - buster
        - stable
        - testing
    - name: EL (CentOS)
      versions:
        - 8
        - 7
    - name: windows
      version:
        - 2008x64 (7 64bit)
        - 2008x86 (7 32bit)
        - 2019 (10 64bit)
```

## Requirements

min_ansible_version: 2.7

## Role Variables

```yaml
---
#--- VirtualBox Edition ---#

# http://download.virtualbox.org/virtualbox/

virtualbox_edition: latest-stable
# virtualbox_edition: latest-beta
# virtualbox_edition: latest

#--- VirtualBox checksum algorithm ---#

virtualbox_checksum_algorithm: sha256
# virtualbox_checksum_algorithm: md5

#--- Other --#

# If you *NOT* use apt-cacher-ng or other caching proxy - select "https".
http_or_https: http
# http_or_https: https
```

## Dependencies

Version one:

```bash
cd /path/to/you/ansible/roles
git clone https://github.com/don-rumata/ansible-role-install-virtualbox
```

Version two:

```bash
ansible-galaxy install don_rumata.ansible_role_install_virtualbox
```

### If you want deploy to Windows 7

Download and install [Windows Management Framework 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616)

## HowTo

Quick config WinRM for Windows: <https://ru.stackoverflow.com/a/949971/191416>

## Example Playbook

Install latest stable VirtualBox on Windows or Linux with Extension Pack:

`install-virtualbox.yml`:

```yaml
- name: Install VirtualBox
  hosts: all
  strategy: free
  serial:
    - "100%"
  roles:
    - ansible-role-install-virtualbox
  tasks:
```

## License

Apache License, Version 2.0

## Author Information

[don Rumata](https://github.com/don-rumata)

## TODO

- Add tests.

- Add support Fedora 32

- Add support openSUSE.

- Add support ArchLinux

- Add uninstall.yml
