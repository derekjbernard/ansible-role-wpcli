Ansible Role for WP-CLI
=======================
[![Build Status](https://travis-ci.org/derekjbernard/ansible-wp-cli.svg?branch=master)](https://travis-ci.org/derekjbernard/ansible-role-wp-cli)

Ansible role for installing [WP-CLI](https://wp-cli.org)

Features:
* Installs WP-CLI
* Installs [WP-CLI packages](https://wp-cli.org/commands/package/)
* "checksum" verification of wp-cli install
* option to download all files to a single "deployment host" vs downloading from internet on every host. Good for hosts with limited or metered internet.

Requirements
------------

Ansbile version 2.2+
UNIX-like environment (OS X, Linux, FreeBSD)
php-cli 5.3.29 or later

Role Variables
--------------

| Variable             | Default     | Comments (type)                                   |
| :---                 | :---        | :---                                              |
| ```wp_cli_phar_get_url``` | ```https://raw.github.com/wp-cli/builds/gh-pages/phar/wp-cli.phar``` | Location of the WP-CLI phar to download |
| ```wp_cli_bin_path``` | ```/usr/bin/{{ wp_cli_bin_command }}``` | Location to store WP-CLI on remote machine |
| ```wp_cli_bin_command``` | wp | WP-CLI Coomand on remote machine | 
| ```wp_cli_packages``` |  | List of WP-CLI Packege for Installing |
| ```wp_cli_phar_hash_file_type``` |  | hash type (md5 or sha512) to retrieve for get_url checksum verification of wp-cli.phar |
| ```wp_cli_phar_hash_type_uri_url``` | ```{{ wp_cli_phar_get_url + '.' + wp_cli_phar_hash_file_type }}``` | hash url for verifing wp-cli.phar |
| ```wp_cli_phar_get_url_checksum``` |  | use defined checksum value instead of retrieving it. |
| ```wp_cli_delegate_host``` |  | download wp-cli to specified host and deploy from there |
| ```wp_cli_delegate_host_download_dir``` |  | specify download dir on deployment host |
| ```wp_cli_delegate_host_bin_command``` | ```WP_CLI_PACKAGES_DIR={{ wp_cli_delegate_host_download_dir }}/wp-cli/packages/ {{ wp_cli_bin_path }}``` |  custom command for deployment host |

Dependencies
------------

None

Example Playbooks
-----------------


### On each host download and install wp-cli
```yml
- hosts: all
  roles:
     - derekjbernard.wp-cli
```

### On each host: fetch wp-cli.phar.sha512 hash and use get_url wp-cli.phar checksum verification.
```yml
- hosts: all
  vars:
    wp_cli_phar_hash_file_type: sha512
  roles:
     - derekjbernard.wp-cli
```

### On 'aws-jumphost' download wp-cli and deploy to aws-web-servers
```yml
- hosts: aws-web-servers
  vars:
    wp_cli_delegate_host: aws-jumphost
    wp_cli_delegate_host_download_dir: "/home/ec2-user/downloads/"
  roles:
     - derekjbernard.wp-cli
```

Credits
-------
I was inspired by [Simon Bärlocher's](https://sbaerlocher.ch) excellent [ansible wp-cli role](https://www.github.com/sbaerlocher/ansible.wp-cli). His wp-cli role was the best wp-cli role project I found. Clean code and working travis tests. It also installs wp-cli packages which AFAIK was unique to his project. I am adding that feature to this project, so I'd like to thank Simon for his great work.

Any code or ideas that resemble Simon Bärlocher's ansible.wp-cli project are copyrighted Simon Bärlocher, 2016, and retain his [MIT LICENSE](https://sbaerlo.ch/licence).

License
-------

MIT

Copyright
---------
Contributors retain copyright of their contributions.

Author(s) / Contributors
------------------------
Anyone submitting a pull request may add themselves to the Author/Contributors list as part of that pull request. 

* [Derek J. Bernard](https://github.com/derekjbernard/)
* [Simon Bärlocher](https://sbaerlocher.ch)

Changelog
---------
