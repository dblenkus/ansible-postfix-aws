ansible-postfix-aws
===================

Ansible role to install Postfix and configure it to send emails through Amazon
SES service.

This role follows official `Amazone documentation for SES`_ with some
modifications to ensure idempotency.

.. _Amazone documentation for SES: http://docs.aws.amazon.com/ses/latest/DeveloperGuide/postfix.html

Requirements
------------

Currently, the role only supports `CentOS`_ and
`Red Hat Enterprise Linux (RHEL)`_ EL7 distribution flavors.

If you need support for other flavors, feel free to `submit a pull request`_.

To use this role, you have to have to have `Amazon AWS account`_ (to create it
follow the online instructions) and `IAM user`_ with (at least) following
policy:

.. code-block:: json

    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": "ses:SendRawEmail",
                "Resource": "*"
            }
        ]
    }

To work properly "from" email has to be verified in SES. If you account is still
in sandbox, all "to" emails also have to be verified. For more informations
check `Amazon's instructions on verifying email addresses`_.

.. _CentOS: https://www.centos.org/

.. _Red Hat Enterprise Linux (RHEL):
  https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux

.. _submit a pull request:
  https://github.com/dblenkus/ansible-postfix/aws/pull/new/master

.. _Amazon AWS account: https://aws.amazon.com/

.. _IAM user:
  http://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html

.. _Amazon's instructions on verifying email addresses:
  http://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-email-addresses.html

Role Variables
--------------

+------------------------------------+----------+-------------------------------------------+-----------+----------------------------------------+
|                Name                |   Type   |                Description                | Mandatory |              Default                   |
+====================================+==========+===========================================+===========+========================================+
| ``postfix_aws_ses_host``           |  string  | Hostname of Amazon SES server.            |     no    | ``email-smtp.eu-west-1.amazonaws.com`` |
+------------------------------------+----------+-------------------------------------------+-----------+----------------------------------------+
| ``postfix_aws_ses_port``           | integer  | Port of Amazon SES server.                |     no    |                ``25``                  |
+------------------------------------+----------+-------------------------------------------+-----------+----------------------------------------+
| ``postfix_aws_amazon_key_id``      |  string  | Amazon Key ID used for sending mails.     |    yes    |                                        |
+------------------------------------+----------+-------------------------------------------+-----------+----------------------------------------+
| ``postfix_aws_amazon_access_key``  |  string  | Amazon secret Access Key for sending      |    yes    |                                        |
|                                    |          | mails.                                    |           |                                        |
+------------------------------------+----------+-------------------------------------------+-----------+----------------------------------------+
| ``postfix_aws_from_email``         |  string  | Default from email.                       |    yes    |                                        |
+------------------------------------+----------+-------------------------------------------+-----------+----------------------------------------+

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

Domen Blenkuš
