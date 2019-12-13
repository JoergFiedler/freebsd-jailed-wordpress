freebsd-jailed-wordpress
========================

[![Build Status](https://travis-ci.org/JoergFiedler/freebsd-jailed-wordpress.svg?branch=master)](https://travis-ci.org/JoergFiedler/freebsd-jailed-wordpress)

This role provides a jailed Joomla server. Server can be managed via SFTP. 

Furthermore Lets Encrypt is used to create and manage server certificates.

Requirements
------------

This role is intent to be used with a fresh FreeBSD installation. There is a
Vagrant Box with providers for VirtualBox and EC2 you may use.

Role Variables
--------------
| Variable | Description | Default |
| :------- | :---------- | :-----: |
|wp_db_host IP address of the DB host. | `''` |
|wp_db_host_password | Password of the user who is allowed to create tables and grant permissions. | 'passwd' |
| wp_db_host_user | The user that is allowed to create tables and grant permissions. | `root` | 
| wp_db_name | | The database name for the WP DB instance. | `wp_{{ wp_server_name_ }}` |
| wp_db_password | The password for the WP DB instance. | `wp_{{ wp_server_name_ }}` |
| wp_db_priv: '{{ wp_db_name }}.*:All' | Privileges given to the WP DB instance user. | `{{ WP_db_name }}.*:All` |
| wp_db_user | The WP DB instance user name. | `wp_{{ wp_server_name_ }}` |
| wp_download_url | The wordpress package (tar.gz) | `https://wordpress.org/latest.tar.gz` |
| wp_server_aliases | Aliases (domain names) that the server should listen to. | `''` |
| wp_server_force_www | Set to `yes` if the server should redirect to `www` subdomain. Add `www.{{ server_name }}` to aliases. | `no` |
| wp_server_home | Server home directory. | `/srv/{{ joomla_server_name }}` |
| wp_server_https_certbundle_file: 'localhost-certbundle.pem' | CA certificate chain. | `localhost-certbundle.pem` |
| wp_server_https_dhparam_file | DH param file. |  `localhost-dhparam.pem` |
| wp_server_https_enabled | Enable HTTPS for this server. Automatic redirect of non-HTTPS request will happen. | `yes` |
| wp_server_https_key_file | The server's private key. | `localhost-key.pem` |
| wp_server_name | The server name (domain name). | `{{ jail_name }}` |
| wp_nginx_pf_redirect | All http(s) traffic will be redirect from host to this jail. | `no` |
| wp_server_php_fastcgi_cache | Set to `off` to disable the fastcgi cache. | `'z_nginx'` |
| wp_server_php_max_requests | Maximum number of request handled by php fpm children. | `1000` |
| wp_server_php_max_children | Maximum number of php fpm children running. | `3` |
| wp_server_php_memory_limit| Memory limit for php fpm. | `'64M'` |
| wp_server_php_upload_max_filesize | Max file upload size accepted. | `'48M'` |
| wp_server_php_post_max_size| Max post request size. | `'46M'` |
| wp_server_syslogd_server | The syslogd server to use for request logging. | `localhost` |
| wp_server_tarsnap_enabled | Backup the server's webroot using Tarsnap. Must be enabled on host level as well. | `no` |
| wp_sftp_authorized_keys | File that should be used as `authorized_keys` file for SFTP. | `''` |
| wp_sftp_port | Port to use for SFTP. | `10022` |
| wp_sftp_uuid | UUID for the SFTP user. | `5000` |
| wp_sftp_user | Name of the SFTP user. | `sftp_{{ joomla_server_name_  truncate(5, True, "", 0) }}` |

Dependencies
------------

- [JoergFiedler.freebsd-jailed-nginx](https://galaxy.ansible.com/JoergFiedler/freebsd-jailed-nginx)

Example Playbook
----------------

    - hosts: all
      become: true
    
      vars:
        host_ioc_release_version: '11.3-RELEASE'
    
      tasks:
        - import_role:
            name: 'JoergFiedler.freebsd-jail-host'
        - include_role:
            name: 'JoergFiedler.freebsd-jailed-mariadb'
          vars:
            jail_freebsd_release: '{{ host_ioc_release_version }}'
            jail_name: 'mariadb'
            jail_net_ip: '10.1.0.5'
        - include_role:
            name: 'JoergFiedler.freebsd-jailed-wordpress'
          tags:
            - wordpress
          vars:
            jail_freebsd_release: '{{ host_ioc_release_version }}'
            jail_name: 'wordpress'
            jail_net_ip: '10.1.0.10'
            wp_db_host: '10.1.0.5'
            wp_db_host_password: 'passwd'
            wp_db_host_user: 'root'
            wp_db_password: 'password'
            wp_nginx_pf_redirect: yes
            wp_server_https_enabled: yes
            wp_server_name: 'localhost'
            wp_sftp_authorized_keys: '~/.vagrant.d/insecure_private_key.pub'
            wp_sftp_port: 10022

License
-------

BSD

Author Information
------------------

If you like it or do have ideas to improve this project, please open an issue
on GitHub. Thanks.
