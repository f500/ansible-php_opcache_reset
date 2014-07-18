php\_opcache\_reset
===================

Reset the PHP [OPcache][1] through [CLI][2] and [FPM][3].

This role can be used as part of a deployment play for PHP applications.

This role uses `php-fpm-cli` (which in turn uses `cgi-fcgi`), which can be installed by using the role `f500.php_fpm_cli`.

Many thanx to [Mathias Leppich][4], who is the author of [php-fpm-cli][5] and gave us the idea to create this role!


Requirements
------------

We require that you have PHP running on either the [Command Line Interface][2] or [FastCGI Process Manager][3].
This role is useless if you have neither of them (for example when only using mod_php for Apache).


Role Variables
--------------

These variables control wether you want to reset the OPcache through CLI or FPM (or both):

    php_opcache_reset_through_cli: true
    php_opcache_reset_through_fpm: true

These variables specify the binaries that are used to run the code that resets the OPcache:

    php_opcache_reset_php_bin: php
    php_opcache_reset_php_fpm_cli_bin: php_fpm_cli

This variable controls to which FPM is connected in order to run the reset code (which only applies to FPM, not CLI):

    php_opcache_reset_sockets: [ /var/run/php5-fpm.sock ]

The following variables control the code that is run, and the messages it produces.
These messages are used to know if the changed something or have failed.
You don't really need to touch these in most cases.

    php_opcache_reset_message_success: OPCACHE_RESET_SUCCESSFULLY
    php_opcache_reset_message_disabled: OPCACHE_DISABLED
    php_opcache_reset_message_not_installed: OPCACHE_NOT_INSTALLED

    php_opcache_reset_code: "echo (function_exists('opcache_reset') ? (opcache_reset() ? '{{ php_opcache_reset_message_success }}' : '{{ php_opcache_reset_message_disabled }}') : '{{ php_opcache_reset_message_not_installed }}');"


Dependencies
------------

This role has no hard dependencies, but does rely on `php-fpm-cli` and `cgi-fcgi` which can be installed by using the role `f500.php_fpm_cli`.

The reason we didn't make this a hard dependency is because the 2 roles are meant to be used in different plays. `f500.php_fpm_cli` is for a provisioning play, this role is for a deployment play.


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

LGPLv3

Author Information
------------------

Jasper N. Brouwer, jasper@nerdsweide.nl


[1]: http://php.net/manual/en/book.opcache.php
[2]: http://php.net/manual/en/features.commandline.php
[3]: http://php.net/manual/en/book.fpm.php
[4]: https://github.com/muhqu
[5]: https://gist.github.com/muhqu/91497df3a110f594b992
