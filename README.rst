ansible-postfix-aws
===================

Ansible role to install Postfix and configure it to send emails through Amazon
SES service.

This role follows official `Amazone documentation for SES`_ with some
modifications to ensure idempotency.

.. _Amazone documentation for SES: http://docs.aws.amazon.com/ses/latest/DeveloperGuide/postfix.html

Requirements
------------

Currently, the role only supports `CentOS`_ , `Red Hat Enterprise Linux (RHEL)`_ EL7, `Ubuntu`_ 18.04 LTS distribution flavors.

If you need support for other flavors, feel free to `submit a pull request`_.

To use this role, you have to have to have `Amazon AWS account`_ (to create it
follow the online instructions) and IAM user for SMTP authentication with SES.
To create it, follow the online instructions on `Obtaining SMTP Credentials`_.
Note that the obtained SMTP user name and password are not the same as IAM
user's access key ID and secret access key.

For the role to work properly, the "from" email address has to be verified with
SES. If your account is still in sandbox, all "to" emails also have to be
verified. For more information, check
`Amazon's instructions on verifying email addresses`_.

.. _CentOS: https://www.centos.org/

.. _Red Hat Enterprise Linux (RHEL):
  https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux

.. _submit a pull request:
  https://github.com/dblenkus/ansible-postfix/aws/pull/new/master

.. _Amazon AWS account: https://aws.amazon.com/

.. _Obtaining SMTP Credentials:
  https://docs.aws.amazon.com/ses/latest/DeveloperGuide/smtp-credentials.html

.. _Amazon's instructions on verifying email addresses:
  http://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-email-addresses.html

.. _Ubuntu:
  https://ubuntu.com/

Role Variables
--------------

+---------------------------------------+----------+-------------------------------------------+-----------+-------------------------------------------------------------------------+
|                Name                   |   Type   |                Description                | Mandatory |              Default                                                    |
+=======================================+==========+===========================================+===========+=========================================================================+
| ``postfix_aws_ses_host``              |  string  | Hostname of Amazon SES server.            |     no    | ``email-smtp.eu-west-1.amazonaws.com``                                  |
+---------------------------------------+----------+-------------------------------------------+-----------+-------------------------------------------------------------------------+
| ``postfix_aws_ses_port``              | integer  | Port of Amazon SES server.                |     no    |                ``25``                                                   |
+---------------------------------------+----------+-------------------------------------------+-----------+-------------------------------------------------------------------------+
| ``postfix_aws_ses_username``          |  string  | Username for SMTP authentication with     |    yes    |                                                                         |
|                                       |          | Amazon SES server.                        |           |                                                                         |
+---------------------------------------+----------+-------------------------------------------+-----------+-------------------------------------------------------------------------+
| ``postfix_aws_ses_password``          |  string  | Password for SMTP authentication with     |    yes    |                                                                         |
|                                       |          | Amazon SES server.                        |           |                                                                         |
+---------------------------------------+----------+-------------------------------------------+-----------+-------------------------------------------------------------------------+
| ``postfix_aws_default_from_email``    |  string  | Default From email address.               |    yes    |                                                                         |
+---------------------------------------+----------+-------------------------------------------+-----------+-------------------------------------------------------------------------+
| ``postfix_aws_sender_canonical_maps`` |  list    | List of canonical mappings for envelope   |     no    | .. code-block:: yaml                                                    |
|                                       |          | and header sender addresses of the form:  |           |                                                                         |
|                                       |          |                                           |           |     pattern: "/.+"                                                      |
|                                       |          | .. code-block:: yaml                      |           |     address: "{{ postfix_aws_default_from_email }}"                     |
|                                       |          |                                           |           |     comment: Map all sender addresses to the default From email address |
|                                       |          |     pattern: string                       |           |                                                                         |
|                                       |          |     address: string                       |           |                                                                         |
|                                       |          |     comment: string                       |           |                                                                         |
|                                       |          |                                           |           |                                                                         |
|                                       |          | where ``pattern`` represents a regular    |           |                                                                         |
|                                       |          | expression that matches the original      |           |                                                                         |
|                                       |          | sender address and ``address`` represents |           |                                                                         |
|                                       |          | the sender address with which to replace  |           |                                                                         |
|                                       |          | the original one.                         |           |                                                                         |
|                                       |          | For more information, see `Postfix's      |           |                                                                         |
|                                       |          | postconf.5 manual page`_.                 |           |                                                                         |
|                                       |          | The ``comment`` represent an optional     |           |                                                                         |
|                                       |          | text to put as a comment in the           |           |                                                                         |
|                                       |          | ``/etc/postfix/sender_canonical`` file.   |           |                                                                         |
+---------------------------------------+----------+-------------------------------------------+-----------+-------------------------------------------------------------------------+

.. _Postfix's postconf.5 manual page:
  http://www.postfix.org/postconf.5.html#sender_canonical_maps

Dependencies
------------

No dependencies.

Example Playbook
----------------

To use this role add this to your playbook:

.. code-block:: yaml

    - hosts: servers
      become: true
      roles:
         - { role: dblenkus.postfix-aws }

License
-------

Licensed under the GPLv3 License. See the COPYING file for details.

Author Information
------------------

| Domen Blenkuš
| Tadej Janež
