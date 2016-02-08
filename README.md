freebsd-jailed-joomla
=========

This role provides a jailed Wordpress server. The jail is set up using the latest version.

Additionally an user is created who is allowed to access this site via SFTP.

To see this role in action, have a look at [this project of mine](https://github.com/JoergFiedler/freebsd-ansible-demo).

Requirements
------------

This role is intent to be used with a fresh FreeBSD 10.2 installation. There is a Vagrant Box with providers for VirtualBox and EC2 you may use.

You will find a sample project which uses this role [here](https://github.com/JoergFiedler/freebsd-ansible-demo).

Role Variables
--------------

##### wp_download_url
The Download URL. Default: `'https://wordpress.org/latest.tar.gz'`.

##### wp_db_name
The MariaDB database name. The database will be created if not exists. Default: : Default: `'wp_{{ server_name_ }}'`.

##### wp_db_user
The db user name for this Wordpress installtion. Default: `'wp_{{ server_name_ }}'`.

##### wp_db_password
The db user's password. Default: `'wp_{{ server_name_ }}'`.

##### wp_db_priv
The priviliges granted to the db user. Default: `'{{ wp_db_name }}.*:All'`.

##### wp_db_host
The MariaDB host ip. Default: `'10.1.0.4'`.

##### wp_db_host_user
The administrative DB user used to create the user and the db. Default: `'root'`.

##### wp_db_host_password
The passwort for the administrative DB user. Default: `'passwd'`.

Dependencies
------------

- [JoergFiedler.freebsd-jailed-php-fpm](https://galaxy.ansible.com/JoergFiedler/freebsd-jailed-php-fpm)
- [JoergFiedler.freebsd-jailed-sftp](https://galaxy.ansible.com/JoergFiedler/freebsd-jailed-sftp)

Example Playbook
----------------

    - { role: JoergFiedler.freebsd-jailed-wordpress,
        tags: ['wordpress'],
        use_ssmtp: true,
        use_syslogd_server: true,
        jail_name: 'wordpress',
        jail_net_ip: '10.1.0.6' }

License
-------

BSD

Author Information
------------------

If you like it or do have ideas to improve this project, please open an issue on Github. Thanks.
