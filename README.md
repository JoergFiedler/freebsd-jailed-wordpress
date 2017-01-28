freebsd-jailed-wordpress
========================

This role provides a jailed WordPress server. The jail is set up using the 
latest version. 

Additionally an user is created who is allowed to access this site via SFTP. 

Furthermore Lets Encrypt is used to create and manage server certificates.

To see this role in action, have a look at 
[this project of mine](https://github.com/JoergFiedler/freebsd-ansible-demo).

Requirements
------------

This role is intent to be used with a fresh FreeBSD installation. There is a
Vagrant Box with providers for VirtualBox and EC2 you may use.

You will find a sample project which uses this role 
[here](https://github.com/JoergFiedler/freebsd-ansible-demo).

Role Variables
--------------

##### wp_server_name
The server name for this WordPress installation. Use the domain name here, e.g.
`example.com`. Default: `{{ jail_name }}`.
 
##### wp_server_aliases
The domain name aliases for this server, e.g. `www.example.com`. Default: `''`.

##### wp_server_force_www
Create redirect rules to redirect all requests to `www` subdomain. You need to
specify `wp_server_aliases`. Default: `false`.

##### wp_server_force_https
Create redirect rules to redirect all requests to `https` scheme. Certificates
will be created using Lets Encrypt. Default: `false`.

##### wp_download_url
The Download URL. Default: `'https://wordpress.org/latest.zip'`.

##### wp_db_name
The MariaDB database name. The database will be created if not exists.
Default: : Default: `'wp_{{ wp_server_name_ }}'`.

##### wp_db_user
The db user name for this WordPress installation. Default: `'wp_{{ wp_server_name_ }}'`.

##### wp_db_password
The db user's password. Default: `'wp_{{ server_name_ }}'`.

##### wp_db_priv
The privileges granted to the db user. Default: `'{{ wp_db_name }}.*:All'`.

##### wp_db_host
The MariaDB host ip. Default: `''`.

##### wp_db_host_user
The administrative DB user used to create the user and the db. Default: `''`.

##### wp_db_host_password
The password for the administrative DB user. Default: `''`.

##### wp_sftp_uuid
The uid for the SFTP user to create. Default: `5000`.

##### wp_sftp_port
The port the SFTP server should listen on. Default: `10200`.

##### wp_sftp_user
The SFTP user name. Default: `'sftp_{{ wp_server_name_ | truncate(5, True, "") }}'`.

##### wp_sftp_authorized_keys
The authorized key file used for SFTP access to this WordPress installation. 
Default: `''`

Dependencies
------------

- [JoergFiedler.freebsd-jailed-nginx](https://galaxy.ansible.com/JoergFiedler/freebsd-jailed-nginx)

Example Playbook
----------------

    - { role: JoergFiedler.freebsd-jailed-wordpress,
        tags: ['_wordpress'],
        use_ssmtp: true,
        use_syslogd_server: true,
        jail_name: 'wordpress',
        jail_net_ip: '10.1.0.6',
        wp_server_name: 'example.com',
        wp_server_aliases: 'www.example.com',
        wp_db_host_user: 'root',
        wp_db_host_password: 'passwd'
        wp_db_host: 'mariadb.host',
        wp_sftp_authorized_keys: 'public.key.file' }

License
-------

BSD

Author Information
------------------

If you like it or do have ideas to improve this project, please open an issue
on GitHub. Thanks.
