debops.boxbackup
################



`BoxBackup`_ is an automated, centralized, encrypted backup service. This
role will install and configure the server on specified host and then
configure all specified clients to create backup on the
``boxbackup-server`` host.

.. _BoxBackup: http://boxbackup.org/

.. contents:: Table of Contents
   :local:
   :depth: 2
   :backlinks: top

Installation
~~~~~~~~~~~~

This role requires at least Ansible ``v1.7.0``. To install it, run::

    ansible-galaxy install debops.boxbackup


Role dependencies
~~~~~~~~~~~~~~~~~

- ``debops.ferm``
- ``debops.etc_services``
- ``debops.secret``


Role variables
~~~~~~~~~~~~~~

List of default variables available in the inventory::

    ---
    
    # FQDN address of the Box Backup server to use
    boxbackup_server: '{{ hostvars[groups.debops_boxbackup[0]]["ansible_fqdn"] }}'
    
    # Set to list of IP addresses / network ranges to allow access only from these
    # networks. Empty list allows access from any host / network.
    boxbackup_allow: []
    
    # Directory where boxbackup-server is storing backups
    boxbackup_storage: '/srv/boxbackup'
    
    # boxbackup-server is listening on this IP address (all interfaces by default)
    boxbackup_listenAddresses: '0.0.0.0'
    
    # Enable/Disable verbose logging
    boxbackup_verbose: 'no'
    
    # 32-bit hexadecimal number representing the boxbackup-client account on the server
    # By default it is computed automatically as: 'ansible_fqdn | sha1sum | cut -c1-8'
    boxbackup_account: ""
    
    # Soft limit for storage space in megabytes, by default it's calculated as
    # total disk space of a given host. When used space is bigger than this,
    # boxbackup-server starts to remove old and deleted data
    boxbackup_softlimit:
    
    # Hard limit for storage space in megabytes. by default it's calculated as
    # soft limit * multiplier (see below). When used space reaches this limit,
    # server refuses to accept new data
    boxbackup_hardlimit:
    
    # Additional disk space added to soft limit, in megabytes. If this number is
    # negative, you will substract given amount of disk space from calculated soft
    # limit
    boxbackup_softlimit_padding: 1024
    
    # Hard limit multiplier will by default set hard limit to equal
    # soft limit + 50%. If you set this number lower than 1.0, you will have
    # smaller hard limit than soft limit, which is not a good idea
    boxbackup_hardlimit_multiplier: 1.5
    
    # Email address which will receive alerts from boxbackup. By default it's
    # <backup@localhost>, which is usually aliased to root account
    boxbackup_email: 'backup'
    
    # List of directories to back up; directory is a hash key, optional
    # exclude/include directives should be written as a text block. Examples can be
    # found in the /etc/boxbackup/bbackupd.conf config file
    boxbackup_locations:
      '/etc': |
        ExcludeFile = /etc/boxbackup/bbackupd/{{ boxbackup_account }}-FileEncKeys.raw
    
      '/home':
    
      '/opt':
    
      '/root':
    
      '/srv':
    
      '/usr/local':
    
      '/var': |
        ExcludeDir = /var/spool/postfix/dev
    
    # List of additional directories / mount points to back up, format is the same
    # as a list above
    boxbackup_locations_custom:

List of internal variables used by the role::

    boxbackup_account
    boxbackup_hardlimit
    boxbackup_softlimit


Authors and license
~~~~~~~~~~~~~~~~~~~

``debops.boxbackup`` role was written by:

- Maciej Delmanowski | `e-mail <mailto:drybjed@gmail.com>`__ | `Twitter <https://twitter.com/drybjed>`__ | `GitHub <https://github.com/drybjed>`__

License: `GPLv3 <https://tldrlegal.com/license/gnu-general-public-license-v3-%28gpl-3%29>`_

