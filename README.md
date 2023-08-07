# Ansible Role: Install VirtualBox

[![License][license-image]][license-url] [![Ansible Galaxy][ansible-galaxy-image]][ansible-galaxy-url] [![Ansible Galaxy Quality][ansible-galaxy-quality-image]][ansible-galaxy-url] [![Ansible Galaxy Release][ansible-galaxy-release-image]][ansible-galaxy-url]

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
        # - 7
    - name: opensuse
      vesrion:
        - tumbleweed
        - 15.3
    - name: windows
      version:
        - 2008x64 (7 64bit)
        - 2008x86 (7 32bit)
        - 2019 (10 64bit)
```

## Requirements

min_ansible_version: 2.9

## Role Variables

```yaml
---
#--- VirtualBox Edition ---#

# http://download.virtualbox.org/virtualbox/

virtualbox_edition: latest-stable
# virtualbox_edition: latest-beta
# virtualbox_edition: latest

#--- VirtualBox repos ---#

virtualbox_repo_deb_key:
  - https://www.virtualbox.org/download/oracle_vbox.asc
  - https://download.virtualbox.org/virtualbox/debian/oracle_vbox_2016.asc
# virtualbox_repo_deb_key:
#   - http://10.10.10.10/soft/virtualbox/oracle_vbox.asc
#   - http://10.10.10.10/soft/virtualbox/oracle_vbox_2016.asc

virtualbox_repo_rpm_key: https://www.virtualbox.org/download/oracle_vbox.asc
# virtualbox_repo_rpm_key: http://10.10.10.10/soft/virtualbox/oracle_vbox.asc

#--- VirtualBox api ---#

virtualbox_url_prefix: 'https://download.virtualbox.org/virtualbox'

virtualbox_url_version: '{{ virtualbox_url_prefix }}/{{ virtualbox_edition | upper }}.TXT'
# virtualbox_url_version: http://10.10.10.10/soft/virtualbox/latest-stable.txt

virtualbox_url_path_to_files: '{{ virtualbox_url_prefix }}/{{ virtualbox_available_version_fact }}'

virtualbox_checksum_file_url: '{{ virtualbox_url_path_to_files }}/{{ virtualbox_checksum_algorithm | upper }}SUMS'
# virtualbox_checksum_file_url: http://10.10.10.10/soft/virtualbox/latest-stable/SHA256SUMS

virtualbox_windows_local_download_path: '{{ ansible_env.TMP }}\virtualbox'

virtualbox_install_extension_pack: true
# virtualbox_install_extension_pack: false

# If the variable is defined - virtualbox will be downloaded from the specified location (Windows only)
# virtualbox_direct_download_url: http://10.10.10.10/soft/virtualbox/latest-stable/virtualbox-latest.exe

# If the variable is defined - extension pack will be downloaded from the specified location
# virtualbox_extpack_direct_download_url: http://10.10.10.10/soft/virtualbox/latest-stable/oracle_vm_virtualbox_extension_pack-latest.vbox-extpack

virtualbox_checksum_algorithm: sha256
# virtualbox_checksum_algorithm: md5

virtualbox_version: latest
# virtualbox_version: 5.2.38

#--- Other --#

# If you *NOT* use apt-cacher-ng or other caching proxy - select "https".
http_or_https: http
# http_or_https: https
```

## Dependencies

### If you want deploy to Windows 7

Download and install [Windows Management Framework 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616)

## HowTo

Quick config WinRM for Windows: <https://ru.stackoverflow.com/a/949971/191416>

### How to install role

Over `ansible-galaxy`:

```bash
ansible-galaxy install don_rumata.ansible_role_install_virtualbox
```

Over `bash+git`:

```bash
mkdir -p "$HOME/.ansible/roles"
cd "$HOME/.ansible/roles"
git clone https://github.com/don-rumata/ansible-role-install-virtualbox don_rumata.ansible_role_install_virtualbox
```

## Example Playbook

### I

Install latest stable `VirtualBox` on Windows or Linux with `Extension Pack`:

`install-virtualbox.yml`:

```yaml
- name: Install VirtualBox
  hosts: all
  strategy: free
  serial:
    - "100%"
  roles:
    - don_rumata.ansible_role_install_virtualbox
  tasks:
```

### II

Install `VirtualBox` and `Extension Pack` from local web server:

`install-virtualbox.yml`:

```yaml
---
  - name: Install VirtualBox
    hosts: all
    strategy: free
    serial:
      - "100%"
    roles:
      - role: ansible-role-install-virtualbox
        virtualbox_repo_deb_key:
          - http://10.10.10.10/soft/virtualbox/oracle_vbox.asc
          - http://10.10.10.10/soft/virtualbox/oracle_vbox_2016.asc
        virtualbox_repo_rpm_key: http://10.10.10.10/soft/virtualbox/oracle_vbox.asc
        virtualbox_url_version: http://10.10.10.10/soft/virtualbox/latest-stable.txt
        virtualbox_checksum_file_url: http://10.10.10.10/soft/virtualbox/latest-stable/SHA256SUMS
        virtualbox_extpack_direct_download_url: http://10.10.10.10/soft/virtualbox/latest-stable/oracle_vm_virtualbox_extension_pack-latest.vbox-extpack
        virtualbox_direct_download_url: http://10.10.10.10/soft/virtualbox/latest-stable/virtualbox-latest.exe
    tasks:
```

## License

Apache License, Version 2.0

## Author Information

[don Rumata](https://github.com/don-rumata)

## TODO

- Add tests.

- Add support Fedora

- ~~Add support openSUSE~~

- Add support ArchLinux

- Add uninstall.yml

[license-image]: https://img.shields.io/github/license/don-rumata/ansible-role-install-virtualbox.svg
[license-url]: https://opensource.org/licenses/Apache-2.0

[ansible-galaxy-image]: https://img.shields.io/badge/ansible_galaxy-don__rumata.ansible__role__install__virtualbox-blue.svg
[ansible-galaxy-url]: https://galaxy.ansible.com/don_rumata/ansible_role_install_virtualbox

[ansible-galaxy-quality-image]: https://img.shields.io/ansible/quality/62939

[ansible-galaxy-release-image]: https://img.shields.io/github/v/release/don-rumata/ansible-role-install-virtualbox.svg?include_prereleases
