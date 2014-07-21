php\_opcache\_reset
===================

Reset the PHP OPcache that lives in PHP FPM's memory (without the need of a webserver).

This role can be used as part of a deployment play for PHP applications.

This role uses `php-fpm-cli` (which in turn uses `cgi-fcgi`), which can be installed by using the role [`f500.php_fpm_cli`][3].

Many thanx to [Mathias Leppich][1], who is the author of [php-fpm-cli][2] and gave us the idea to create this role!


Requirements
------------

We require that you have PHP FPM up and running.

The PHP FPM socket(s) must be readable and writable by the ansible user that will run this role.
A commen way to ensure this is to add the ansible user to the group set by the PHP FPM directive `listen.group`,
and use `0660` permissions on the socket (set by the PHP FPM directive `listen.mode`).


Role Variables
--------------

This variable specifies the name/location of the `php-fpm-cli` tool:

    php_opcache_reset_binary: php-fpm-cli

This variable controls to which FPM is connected in order to run the `opcache_reset()` code:

    php_opcache_reset_sockets: [ /var/run/php5-fpm.sock ]

The following variables control the code that is run, and the messages it produces.
These messages are used to know if the task changed something or has failed.
You don't really need to touch these in most cases.

    php_opcache_reset_message_success: OPCACHE_RESET_SUCCESSFULLY
    php_opcache_reset_message_disabled: OPCACHE_DISABLED

    php_opcache_reset_code: "echo (opcache_reset() ? '{{ php_opcache_reset_message_success }}' : '{{ php_opcache_reset_message_disabled }}');"


Dependencies
------------

This role has no hard dependencies, but does rely on `php-fpm-cli` (and `cgi-fcgi`) which can be installed by using the role [`f500.php_fpm_cli`][3].

The reason we didn't make this a hard dependency is because the 2 roles are meant to be used in different plays.
`f500.php_fpm_cli` is for a provisioning play, this role is for a deployment play.


Example Playbook
----------------

Use as is:

    - hosts: servers
      roles:
         - { role: f500.php_opcache_reset }

Use on multiple FPM sockets:

    - hosts: servers
      roles:
         - { role: f500.php_opcache_reset, php_opcache_reset_sockets: [ /var/run/php5-fpm-main.sock, /var/run/php5-fpm-dev.sock ]}


License
-------

[LGPLv3][4]


Author Information
------------------

Jasper N. Brouwer, jasper@future500.nl

Ramon de la Fuente, ramon@future500.nl


[1]: https://github.com/muhqu
[2]: https://gist.github.com/muhqu/91497df3a110f594b992
[3]: https://galaxy.ansible.com/list#/roles/1147
[4]: https://github.com/f500/ansible-php_opcache_reset/blob/master/COPYING.LESSER
