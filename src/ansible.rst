Ansible
=======

.. contents:: Table of Contents

Introduction
------------

Ansible is a simple utility for automating system administration tasks
via SSH for UNIX-like operating systems. The only requirements are a SSH
connection from a control node to a managed node and Python on both
nodes. Ansible uses YAML syntax and does not require any knowledge of
programming. [1]

There is also support for Windows modules. Ansible is executed on a
control node that runs on Linux, using Python. A remote connection to
WinRM (via HTTPS, by default) is made and then modules are executed
remotely using PowerShell commands. [33]

The official documentation can be found here:

-  Latest stable: http://docs.ansible.com/ansible/latest/index.html
-  Development: http://docs.ansible.com/ansible/devel/index.html

Editions
~~~~~~~~

There are two editions of Ansible available. There is the upstream
Ansible community project that gets frequent updates and there is also
Red Hat Ansible Engine, which is an enterprise solution. Ansible Engine
is designed to provide a downstream version that is more stable, secure,
and reliable. Support is provided that includes covers Core modules,
priority bug and feature updates, documentation, and more. [42]

Installation
------------

The Ansible community edition 2.4 requires Python 2.6, 2.7, or >= 3.5 on
both the control and managed nodes. [1] Python 3 support is still in
development but should be stable within the next few releases. [19]

RHEL:

.. code-block:: sh

    $ sudo yum install epel-release
    $ sudo yum install ansible

Debian:

.. code-block:: sh

    $ sudo apt-get install software-properties-common
    $ sudo apt-add-repository ppa:ansible/ansible
    $ sudo apt-get update
    $ sudo apt-get install ansible

Source code:

.. code-block:: sh

    $ git clone git://github.com/ansible/ansible.git
    $ cd ./ansible/
    $ git branch -a | grep stable
    $ git checkout remotes/origin/stable-2.4
    $ git submodule update --init --recursive
    $ source ./hacking/env-setup

Updating source code installations:

.. code-block:: sh

    $ git pull --rebase
    $ git submodule update --init --recursive

[18]

For managing Windows servers, the "winrm" Python library is required on
the Ansible control node. The remote Windows servers need PowerShell >=
3.0 installed and WinRM enabled. [33]

Configuration
-------------

Main
~~~~

All of the possible configuration files are listed below in the order
that they are read. The last file overrides any previous settings.

Configuration files:

-  ``$ANSIBLE_CONFIG`` = A command line variable containing the Ansible
   configuration settings.
-  ``ansible.cfg`` = If it is in the current directory, it will be used.
-  ``~/.ansible.cfg`` = The configuration file in a user's home
   directory.
-  ``/etc/ansible/ansible.cfg`` = The global configuration file.

Common settings:

-  [default]

   -  ansible\_managed = String. The phrase that will be assigned to the
      ``{{ ansible_managed }}`` variable. This should generally reside
      at the top of a template file to indicate that the file is managed
      by Ansible.
   -  ask\_pass = Boolean. Default: False. Prompt the user for the SSH
      password.
   -  ask\_sudo\_pass = Boolean. Default: False. Prompt the user for the
      sudo password.
   -  ask\_vault\_pass = Boolean. Default: False. Prompt the user for
      the Ansible vault password.
   -  command\_warnings = Boolean. Default: True. Inform the user an
      Ansible module can be used instead of running certain commands.
   -  deprecation\_warnings = Boolean. Default: True. Show deprecated
      messages about features that will be removed in a future release
      of Ansible.
   -  display\_skipped\_hosts = Boolean. Default: True. Show tasks that
      a skipped host would have run.
   -  executable = String. Default: /bin/bash. The shell executable to
      use.
   -  forks = Integer. Default: 5. The number of parallel processes used
      to run tasks on remote hosts. This is not how many hosts a
      Playbook or module can run on, that is handled by the "serial"
      module. This helps to increase the performance of many operations
      across a large number of remote hosts.
   -  host\_key\_checking = Boolean. Default: True. Do not automatically
      accept warnings about leaving SSH fingerprints on a connection to
      a new host.
   -  internal\_poll\_interval = Float. Default: 0.001. The number of
      seconds to wait before checking on the status of a module that is
      being executed.
   -  inventory = String. Default: /etc/ansible/hosts. The default
      inventory file to find hosts from.
   -  log\_path = String. Default: none. The file to log Ansible's
      operations.
   -  nocolor. Boolean. Default: 0. Do not format Ansible output with
      color.
   -  nocows = Boolean. Default: 0. If the ``cowsay`` binary is present,
      a Playbook will output information using a cow.
   -  hosts = String. Default: \*. The hosts to run a Playbook on if no
      host is specified. The default is to run on all hosts.
   -  private\_key\_file = String. The private SSH key file to use.
   -  remote\_port = Integer. Default: 22. The SSH port used for remote
      connections.
   -  remote\_tmp = String. Default: ~/.ansible/tmp. The temporary
      directory on the remote server to save information to.
   -  remote\_user = String. Default: root. The default ``ansible_user``
      to use for SSH access.
   -  roles\_path = String. The path to the location of installed roles.
   -  sudo\_exe = String. Default: sudo. The binary to run to execute
      commands as a non-privileged user.
   -  sudo\_user = String. Default: root. The user that sudo should run
      as.
   -  timeout = Integer. Default: 10. The amount of time, in seconds, to
      wait for a SSH connection to a remote host.
   -  vault\_password\_file = String. The default file to use for the
      Vault password.

-  [privilege\_escalation]

   -  become = Boolean. Default: False. This specifies if root level
      commands should be run by a privileged user.
   -  become\_method = String. Default: sudo. The method to run root
      tasks.
   -  become\_user = String. Default: root. The user to change to to run
      root tasks.
   -  become\_ask\_pass = Boolean. Default: False. Ask the end-user for
      a password for the become method.

-  [ssh\_connection]

   -  ssh\_args = String. Additional SSH arguments.
   -  retries = Integer. Default: 0 (keep retrying). How many times
      should an SSH connection attempt to reconnect after a failure.
   -  pipelining = Boolean. Default: False. Ansible modules can be
      combined and sent to the remote host via SSH to help save time and
      improve performance. This is disabled by default because ``sudo``
      accounts usually have the "requiretty" option enabled that is not
      compatible with pipelining.
   -  ansible\_ssh\_executable = String. Default: ssh (found in the
      $PATH environment variable). The path to the ``ssh`` binary.

[29]

Python 3
~~~~~~~~

Python 3 is supported on the control node and managed nodes. For using
Python 3 on the managed nodes, the ``ansible_python_interpreter``
variable needs to be set to reference the path to the managed nodes'
Python 3.

Example:

.. code-block:: sh

    $ /usr/bin/python3 /usr/bin/ansible -e "ansible_python_interpreter=/usr/bin/python3" -m setup localhost

Documentation on how to create Ansible modules for Python 3 with
backwards compatibility with Python 2 can be found here:
http://docs.ansible.com/ansible/latest/dev\_guide/developing\_python3.html

[43]

Command Usage
-------------

Refer to Root Page's "Linux Commands" guide in the "Deployment" section.

Playbooks
---------

Playbooks organize tasks into one or more YAML files. It can be a
self-contained file or a large project organized in a directory.
Official examples can he found here at
https://github.com/ansible/ansible-examples.

Directory Structure
~~~~~~~~~~~~~~~~~~~

A Playbook can be self-contained entirely into one file. However,
especially for large projects, each segment of the Playbook should be
split into separate files and directories.

Layout:

::

    ├── production/
    │   ├── group_vars/
    │   ├── host_vars/
    │   └── inventory
    ├── staging/
    │   ├── group_vars/
    │   ├── host_vars/
    │   └── inventory
    ├── roles/
    │   └── general/
    │       ├── defaults/
    │       │   └── main.yml
    │       ├── files/
    │       ├── handlers/
    │       │   └── main.yml
    │       ├── meta/
    │       │   └── main.yml
    │       ├── tasks/
    │       │   └── main.yml
    │       ├── templates/
    │       └── vars/
    │           └── main.yml
    └── site.yml

Layout Explained:

-  production/ = A directory that contains information about the
   Ansible-controlled hosts and inventory variables. This should be used
   for deploying to live production environments. Alternatively, simple
   Playbooks can use a "production" file to list all of the inventory
   servers there.

   -  group\_vars/ = Group specific variables. A file named "all" can be
      used to define global variables for all hosts.
   -  host\_vars/ = Host specific variables.
   -  inventory = The main "production" inventory file.

-  staging/ = The same as the "production/" directory except this is
   designed for running Playbooks in testing environments.
-  roles/ = This directory should contain all of the different roles.

   -  general/ = A role name. This can be anything.

      -  defaults/ = Define default variables. If any variables are
         defined elsewhere, these will be overridden.

         -  main.yml = Each main.yml file is executed as the first file.
            Additional separation of operations can be split into
            different files that can be accessed via "include:"
            statements.

      -  files/ = Store static files that are not modified.
      -  handlers/ = Specify alias commands that can be called using the
         "notify:" method.

         -  main.yml

      -  meta/ = Specify role dependencies and Playbook information such
         as author, version, etc. These can be other roles and/or
         Playbooks.

         -  main.yml

      -  tasks/

         -  main.yml = The tasks' main file is executed first for the
            entire role.

      -  templates/ = Store dynamic files that will be generated based
         on variables.
      -  vars/ = Define role-specific variables.

         -  main.yml

-  site.yml = This is typically the default Playbook file to execute.
   Any name and any number of Playbook files can be used here to include
   different roles.

Examples:

-  site.yml = This is generally the main Playbook file. It should
   include all other Playbook files required if more than one is used.
   [5]

   .. code-block:: yaml

        ---
        # File: site.yaml
        include: nginx.yml
        include: php-fpm.yml

   .. code-block:: yaml

        ---
        # File: nginx.yml
        -  hosts: webnodes
           roles:
             - common
             - nginx

-  roles/\ ``<ROLENAME>``/vars/main.yml = Global variables for a role.

   .. code-block:: yaml

        ---
        # File: vars/main.yaml
        memcache_hosts:
          - 192.168.1.11
          - 192.168.1.12
          - 192.168.1.13
        ldap_ip: 192.168.10.1

-  group\_vars/ and host\_vars/ = These files define variables for hosts
   and/or groups. Details about this can be found in the
   `Variables <#configuration---inventory---variables>`__ section.

-  templates/ = Template configuration files for services. The files in
   here end with a ".j2" suffix to signify that it uses the Jinja2
   template engine. [1]


   .. code-block:: html

       <html>
           <body>My domain name is {{ domain }}</body>
       </html>

Performance Tuning
~~~~~~~~~~~~~~~~~~

A few configuration changes can help to speed up the runtime of Ansible
modules and Playbooks.

-  ansible.cfg

   -  [defaults]

      -  forks = The number of parallel processes that are spun up for
         remote connections. The default is 5. This should be increased
         to a larger number to handle . The recommended number is
         ``forks = (processor_cores * 5)``. [35]
      -  pipelining = Enable pipelining to bundle commands together that
         do not require a file transfer. This is disabled by default
         because most sudo users are enforced to use the ``requiretty``
         sudo option that pipelining is incompatible with. [28]
      -  gathering = Set this to "explicit" to only gather the necessary
         facts when/if they are required by the Playbook. [29]

Fact caching will help to cache host information. By only gathering the
setup facts information once, this helps to speed up execution time if
Ansible will need to run Playbooks on hosts multiple times. The
supported types of fact caching are currently memory (none), file
(json), and Redis.

All:

-  ansible.cfg

   -  [defaults]

      -  gathering = smart
      -  fact\_caching = 86400

         -  This will set the cache time to 1 day.

File (JSON):

-  ansible.cfg

   -  [defaults]

      -  fact\_caching = jsonfile
      -  fact\_caching\_connection =
         ``<TEMPORARY_DIRECTORY_TO_AUTOMATICALLY_CREATE>``

Redis:

-  ansible.cfg

   -  [defaults]

      -  fact\_caching = redis

         -  As of Ansible 2.3, there is still no way of defining a
            custom IP and/or port of a Redis server. It is assumed to be
            running on localhost with the default port.

[4]

Inventory
---------

Default file: /etc/ansible/hosts

The hosts file is referred to as the "inventory" for Ansible. Here
servers and groups of servers are defined. Ansible can then be used to
execute commands and/or Playbooks on these hosts. There are two groups
that are automatically created by Ansible. The "all" group is every
defined host and "ungrouped" is a group of hosts that do not belong to
any groups. User defined groups are created by using brackets "[" and
"]" to specify the name.

Syntax:

.. code-block:: ini

    <SERVER1NAME> ansible_host=<SERVER1_HOSTNAME>

    [<GROUPNAME>]
    <SERVER1NAME>

Example:

.. code-block:: ini

    [dns-us]
    dns-us01
    dns-us02
    dns-us03

A sequence of letters "[a:z]" or numbers "[0:9]" can be used to
dynamically define a large number of hosts.

Example:

.. code-block:: ini

    [dns-us]
    dns-us[01:03]

A group can also be created from other groups by using the ":children"
tag.

Example:

.. code-block:: ini

    [dns-global:children]
    dns-us
    dns-ca
    dns-mx

Variables are created for a host and/or group using the tag ":vars".
Then any custom variable can be defined and associated with a string. A
host specifically can also have it's variables defined on the same line
as it's Ansible inventory variables. [3] A few examples are listed
below. These can also be defined in separate files as explained in the "Variables" chapter.

Example:

.. code-block:: ini

    examplehost ansible_user=toor ansible_host=192.168.0.1 custom_var_here=True

.. code-block:: ini

    [examplegroup:vars]
    domain_name=examplehost.tld
    domain_ip=192.168.7.7

There are a large number of customizations that can be used to suit most
server's access requirements.

Common inventory options:

-  ansible\_host = The IP address or hostname of the server.
-  ansible\_port = A custom SSH port (i.e., if not using the standard
   port 22).
-  ansible\_connection = These options specify how to log in to execute
   tasks.

   -  chroot = Run commands in a directory using chroot.
   -  local = Run on the local system.
   -  ssh = Run commands over a remote SSH connection (default).
   -  winrm = Use the Windows Remote Management (WinRM) protocols to
      connect to Windows servers.

-  ansible\_winrm\_server\_cert\_validation

   -  ignore = Ignore self-signed certificates for SSL/HTTPS connections
      via WinRM.

-  ansible\_user = The SSH user.
-  ansible\_pass = The SSH user's password. This is very insecure to
   keep passwords in plain text files so it is recommended to use SSH
   keys or pass the "--ask-pass" option to ansible when running tasks.
-  ansible\_ssh\_private\_key\_file = Specify the private SSH key to use
   for accessing the server(s).
-  ansible\_ssh\_common\_args = Append additional SSH command-line
   arguments for sftp, scp, and ssh.
-  ansible\_{sftp\|scp\|ssh}\_extra\_args = Append arguments for the
   specified utility.
-  ansible\_python\_interpreter = This will force Ansible to run on
   remote systems using a different Python binary. Ansible only supports
   Python 2 so on server's where only Python 3 is available a custom
   install of Python 2 can be used instead. [3]
-  ansible\_vault\_password\_file = Specify the file to read the Vault
   password from. [21]
-  ansible\_become = Set to "true" or "yes" to become a different user
   than the ansible\_user once logged in.

   -  ansible\_become\_method = Pick a method for switching users. Valid
      options are: sudo, su, pbrun, pfexec, doas, or dzdo.
   -  ansible\_become\_user = Specify the user to become.
   -  ansible\_become\_pass = Optionally use a password to change users.
      [13]

Examples:

.. code-block:: ini

    localhost ansible_connection=local
    dns1 ansible_host=192.168.1.53 ansible_port=2222 ansible_become=true ansible_become_user=root ansible_become_method=sudo
    dns2 ansible_host=192.168.1.54
    /home/user/ubuntu1604 ansible_connection=chroot

[4]

Production and Staging
~~~~~~~~~~~~~~~~~~~~~~

Ansible best practices suggest having a separation between a production
and staging inventory. Changes should be tested in the staging
environment and then eventually ran on the production server(s).

Scenario #1 - Use the Same Variables

A different inventory file can be created if all of the variables are
the exact same in the production and staging environments. This will run
the same Playbook roles on a different server.

Syntax:

::

    ├── production
    ├── staging
    ├── group_vars
    │   ├── <GROUP>
    ├── host_vars
    │   ├── <HOST>

.. code-block:: sh

    $ ansible-playbook -i production <PLAYBOOK>.yml

.. code-block:: sh

    $ ansible-playbook -i staging <PLAYBOOK>.yml

Example:

::

    ├── production
    ├── staging
    ├── group_vars
    │   ├── web
    │   ├── db
    │   ├── all
    ├── host_vars
    │   ├── web1
    │   ├── web2
    │   ├── db1
    │   ├── db2
    │   ├── db3

Scenario #2 - Use Different Variables

In more complex scenarios, the inventory and variables will be different
in production and staging. This requires further separation. Instead of
using a "production" or "staging" inventory file, they can be split into
directories. These directories contain their own group and host
variables.

Syntax:

::

    ├── production
    │   ├── group_vars
    │   │   ├── <GROUP>
    │   ├── host_vars
    │   │   ├── <HOST>
    │   └── inventory

::

    ├── staging
    │   ├── group_vars
    │   │   ├── <GROUP>
    │   ├── host_vars
    │   │   ├── <HOST>
    │   └── inventory

.. code-block:: sh

    $ ansible-playbook -i production <PLAYBOOK>.yml

.. code-block:: sh

    $ ansible-playbook -i staging <PLAYBOOK>.yml

Example:

::

    ├── production
    │   ├── group_vars
    │   │   ├── web
    │   │   ├── db
    │   │   ├── all
    │   ├── host_vars
    │   │   ├── web1
    │   │   ├── web2
    │   │   ├── db1
    │   │   ├── db2
    │   │   ├── db3
    │   └── inventory

::

    ├── staging
    │   ├── group_vars
    │   │   ├── web
    │   │   ├── db
    │   │   ├── all
    │   ├── host_vars
    │   │   ├── web1
    │   │   ├── web2
    │   │   ├── db1
    │   │   ├── db2
    │   │   ├── db3
    │   └── inventory

[5][22]

Variables
~~~~~~~~~

Variables that Playbooks will use can be defined for specific hosts
and/or groups. The file that stores the variables should reflect the
name of the host and/or group. Global variables can be found in the
``/etc/ansible/`` directory. [3]

Inventory variable directories and files:

-  host\_vars/

  -  ``<HOST>`` = Variables for a host defined in the inventory file.

-  group\_vars/

  -  ``<GROUP>``/

    -  vars = Variables for this group.
    -  vault = Encrypted Ansible Vault variables. [5]

  -  all = This file contains variables for all hosts.
  -  ungrouped = This file contains variables for all hosts that are not defined in any groups.

It is assumed that the inventory variable files are in YAML format. Here
is an example for a host variable file.

Example:

.. code-block:: yaml

    ---
    domain_name: examplehost.tld
    domain_ip: 192.168.10.1
    hello_string: Hello World!

In the Playbook and/or template files, these variables can then be
referenced when enclosed by double braces "{{" and "}}". [4]

Example:

::

    Hello world from {{ domain_name }}!

Variables from other hosts or groups can also be referenced.

Syntax:

::

    {{ groupvars['<GROUPNAME>']['<VARIABLE>'] }}
    {{ hostvars['<HOSTNAME>']['<VARIABLE>'] }}

::

    {{ groupvars.<HOSTNAME>.<VARIABLE>}}
    {{ hostvars.<HOSTNAME>.<VARIABLE> }}

Example:

::

    - command: echo {{ hostvars.db3.hostname }}

The order that variables take precedence in is listed below. The bottom
locations get overridden by anything above them.

-  extra vars
-  task vars
-  block vars
-  role and include vars
-  set\_facts
-  registered vars
-  play vars\_files
-  play vars\_prompt
-  play vars
-  host facts
-  playbook host\_vars
-  playbook group\_vars
-  inventory host\_vars
-  inventory group\_vars
-  inventory vars
-  role defaults

[5]

Vault Encryption
~~~~~~~~~~~~~~~~

Any file in a Playbook can be encrypted. This is useful for storing
sensitive username and passwords securely. A password is used to open
these files after encryption. All encrypted files in a Playbook should
use the same password.

Vault Usage:

-  Create a new encrypted file.

   .. code-block:: sh

       $ ansible-vault create <FILE>.yml

-  Encrypt an existing plaintext file.

   .. code-block:: sh

       $ ansible-vault encrypt <FILE>.yml

-  Viewing the contents of the file.

   .. code-block:: sh

       $ ansible-vault view <FILE>.yml

-  Edit the encrypted file.

   .. code-block:: sh

       $ ansible-vault edit <FILE>.yml

-  Change the password.

   .. code-block:: sh

       $ ansible-vault rekey <FILE>.yml

-  Decrypt to plaintext.

   .. code-block:: sh

       $ ansible-vault decrypt <FILE>.yml

Playbook Usage:

-  Run a Playbook, prompting the user for the Vault password.

   .. code-block:: sh

       $ ansible-playbook --ask-vault-pass <PLAYBOOK>.yml

-  Run the Playbook, reading the file for the vault password.

   .. code-block:: sh

       $ ansible-playbook --vault-password-file <PATH_TO_VAULT_PASSWORD_FILE> <PLAYBOOK>.yml

[21]

Dynamic
~~~~~~~

Dynamic inventory can be used to automatically obtain information about
hosts from various infrastructure platforms and tools. Community
provided scripts be be found here:
https://github.com/ansible/ansible/tree/devel/contrib/inventory.

Modules
-------

Main Modules
~~~~~~~~~~~~

Root Pages refers to generic Playbook-related modules as the "main
modules." This is not to be confused with official naming of "core
modules" which is a mixture of both the main and regular modules
mentioned in this guide.

Assert
^^^^^^

Assert is used to check if one or more statements is True. The module
will fail if any statement returns False. Optionally, a message can be
displayed if any operator comparisons return False.

Syntax:

.. code-block:: yaml

    - assert:
        that:
          - "<VALUE1> <COMPARISON_OPERATOR> <VALUE2>"
        msg: "<MESSAGE>"

Example:

.. code-block:: yaml

    - cmd: /usr/bin/date
      register: date_command
      ignore_errors: True

    - assert:
        that:
          - "date_command.rc == 0"
          - "'2017' in date_command.stdout"
        msg: "Date either failed or did not return the correct year."

[45]

Async
^^^^^

The "async" function can be used to start a detached task on a remote
system. Ansible will then poll the server periodically to see if the
task is complete (by default, it checks every 10 seconds). Optionally a
custom poll time can be set. [15]

Syntax:

.. code-block:: yaml

    async: <SECONDS_TO_RUN>

Example:

.. code-block:: yaml

     - command: bash /usr/local/bin/example.sh
        async: 15
        poll: 5

Block
^^^^^

A ``block`` is used to handle logic for executing tasks. A set of tasks
can be run, for example, if a condition is met. This also handles errors
in a ``try/except`` fashion. If the code from the ``block`` fails then
it proceeds to run the tasks in the ``rescue`` section. There is also a
final ``always`` section that will execute whether the block failed or
not.

Syntax (minimal):

.. code-block:: yaml

    block:

Syntax (full):

.. code-block:: yaml

    block:
      <ACTIONS>
    rescue:
      <ACTIONS>
    always:
      <ACTIONS>

Example:

.. code-block:: yaml

    - name: Installing docker
      block:
        - package:
            name: docker
            state: latest
      rescue:
        - debug:
            msg: "Unable to properly install docker. Cleaning up now."
        - file:
            dest: /path/to/custom/docker/files
            state: absent
      always:
        - debug:
            msg: "Continuing onto the next set of tasks..."

[53]

Check Mode
^^^^^^^^^^

A Playbook can run in a test mode with ``--check``. No changes will be
made. Optionally, the ``--diff`` argument can also be added to show
exactly what would be changed.

Syntax:

.. code-block:: sh

    $ ansible-playbook --check site.yml

.. code-block:: sh

    $ ansible-playbook --check --diff site.yml

In Ansible 2.1, the ``ansible_check_mode`` variable was added to verify
if check mode is on or off. This can be used to forcefully run tasks
even if check mode is on.

Examples:

.. code-block:: yaml

    - command: echo "Hello world"
      when: not ansible_check_mode

.. code-block:: yaml

     - name: Continue if this fails when check_mode is enabled
        stat: path=/etc/neutron/neutron.conf
        register: neutron_conf
        ignore_errors: "{{ ansible_check_mode }}"

In Ansible 2.2, the ``check_mode`` module can be forced to run during a
check mode. [31]

Syntax:

.. code-block:: yaml

    check_mode: no

Example:

.. code-block:: yaml

    - name: Install the EPEL repository
      yum:
        name: epel-release
        state: latest
      check_mode: no

Debug
^^^^^

The debug module is used for helping facilitate troubleshooting. It
prints out specified information to standard output.

Syntax:

.. code-block:: yaml

    debug:

Common options:

-  msg = Display a message.
-  var = Display a variable.
-  verbosity = Show more verbose information. The higher the number, the
   more verbose the information will be. [45]

Example:

-  Print Ansible's hostname of the current server that the script is
   being run on.

.. code-block:: yaml

    debug:
      msg: The inventory host name is {{ inventory_hostname }}

Gather Facts
^^^^^^^^^^^^

By default, Ansible will connect to all hosts related to a Playbook and
cache information about them. This includes hostnames, IP addresses, the
operating system version, etc.

Syntax:

.. code-block:: yaml

    gather_facts: <BOOLEAN>

If these variables are not required then gather\_facts and be set to
"False" to speed up a Playbook's run time. [23]

Example:

.. code-block:: yaml

    gather_facts: False

In other situations, information about other hosts may be required that
are not being used in the Playbook. Facts can be gather about them
before the roles in a Playbook are executed.

Example:

.. code-block:: yaml

    ---
    - hosts: squidproxy1,squidproxy2,squidproxy3
      gather_facts: True

    - hosts: monitor1,monitor2
      roles:
       - common
       - haproxy

Handlers and Notify
^^^^^^^^^^^^^^^^^^^

The ``notify`` function will run a handler defined in the
``handlers/main.yml`` file within a role if the state of the module it's
tied to changes. Optionally, a "listen" directive can be given to
multiple handlers. This will allow them all to be executed at once (in
the order that they were defined). Handlers cannot have the same name,
only the same listen name. This is useful for checking if a
configuration file changed and, if it did, then restart the service.

Handlers only execute when a Playbook successfully completes. For
executing handlers sooner, refer to the "meta" main module's
documentation.

Syntax (handlers/main.yml):

.. code-block:: yaml

    handlers:
      - name: <HANDLER_NAME>
        <MODULE>: <ARGS>
        listen: <LISTEN_HANDLER_NAME>

Syntax (tasks/main.yml):

.. code-block:: yaml

    - <MODULE>: <ARGS>
      notify:
        - <HANDLER_NAME>

Example (handlers/main.yml):

.. code-block:: yaml

    handlers:
      - name: restart nginx
        service: name=nginx state=restarted
        listen: "restart stack"
      - name: restart php-fpm
        service: name=php-fpm state=restarted
        listen: "restart stack"
      - name: restart mariadb
        service: name=mariadb state=restarted
        listen: "restart stack"

Example (tasks/main.yml):

.. code-block:: yaml

    - template: src=nginx.conf.j2 dest=/etc/nginx/nginx.conf
      notify: restart stack

[2]

Meta
^^^^

The meta module handles some aspects of the Ansible Playbooks execution.

All options (free form):

-  clear\_facts = Removes all of the gathered facts about the Playbook
   hosts.
-  clear\_host\_errors = Removes hosts from being in a failed state to
   continue running the Playbook.
-  end\_play = End the Playbook instantly and mark it as successfully
   unless there were any failures.
-  flush\_handlers = Any handlers that have been notified will be run.
-  noop = Do no operations. This is mainly for Ansible developers and
   debugging purposes.
-  refresh\_inventory = Reload the inventory files. This is useful when
   using dynamic inventory scripts.
-  reset\_connection = Closes the current connections to the hosts and
   start a new connection.

Syntax:

.. code-block:: yaml

    meta:

Example:

.. code-block:: yaml

    meta: flush_handlers

[45]

Pause
^^^^^

The ``pause`` module is used to temporarily pause an entire Playbook. If
no time argument is specified, the end-user will need to hit ``CTRL+c``
then ``c`` to continue or hit ``CTRL+c`` and then ``a`` to abort the
Playbook.

All options:

-  minutes
-  prompt = An optional text to display to the end-user.
-  seconds

Syntax:

.. code-block:: yaml

    pause:

Example:

.. code-block:: yaml

    - pause:
        minutes: 3
        prompt: "The new program needs to finish initializing."

[45]

Roles
^^^^^

A Playbook consists of roles. Each role that needs to be run needs to be
specified in a list. Additional roles can be added within a role
dynamically or statically using "include\_role" or "import\_role." [49]

Syntax:

.. code-block:: yaml

    roles:
      - <ROLE1>
      - <ROLE2>

Example:

.. code-block:: yaml

    roles:
      - common
      - httpd
      - sql

Run Once
^^^^^^^^

In some situations a command should only need to be run on one node. An
example is when using a MariaDB Galera cluster where database changes
will get synced to all nodes.

Syntax:

.. code-block:: yaml

    run_once: True

This can also be assigned to a specific host.

Syntax:

.. code-block:: yaml

    run_once: True
    delegate_to: <HOST>

[14]

Serial
^^^^^^

By default, Ansible will only run tasks on 5 hosts at once. This limit
can be modified to run on a different number of hosts or a percentage of
the amount of hosts. This is useful for running Playbooks on a large
amount of servers. [14]

Syntax:

.. code-block:: yaml

    serial: <NUMBER_OR_PERCENTAGE>

Example:

.. code-block:: yaml

    - hosts: web
      tasks:
        - name: Installing Nginx
          package: name=nginx state=present
          serial: 50%

Strategy
^^^^^^^^

By default, a Playbook strategy is set to "linear" meaning that it will
only move onto the next task once it completes on all hosts. This can be
changed to "free" so that once a task completes on a host, that host
will instantly move onto the next available task.

Syntax:

.. code-block:: yaml

    strategy: free

Example (site.yml):

.. code-block:: yaml

    - hosts: all
      strategy: free
      roles:
        - gitlab

[38]

Tags
^^^^

Each task in a tasks file can have a tag associated to it. This should
be appended to the end of the task. This is useful for debugging and
separating tasks into specific groups. Here is the syntax:

Syntax:

.. code-block:: yaml

    tags:
     - <TAG1>
     - <TAG2>
     - <TAG3>

Run only tasks that include specific tags.

.. code-block:: sh

    $ ansible-playbook --tags "<TAG1>,<TAG2>,<TAG3>"

Alternatively, skip specific tags.

.. code-block:: sh

    $ ansible-playbook --skip-tags "<TAG1>,<TAG2>,<TAG3>"

Example:

.. code-block:: yaml

    ---
    # File: webserver.yaml
     - package: name=nginx state=latest
       tags:
        - yum
        - rpm
        - nginx

.. code-block:: sh

    $ ansible-playbook --tags "yum" site.yml webnode1

[8]

Tasks
^^^^^

Playbooks can include specific task files or define and run tasks in the
Playbook file itself. In Ansible 2.0, loops, variables, and other
dynamic elements now work correctly.

Syntax:

.. code-block:: yaml

    - hosts: <HOSTS>
      tasks:
       - <MODULE>:

Example:

.. code-block:: yaml

     - hosts: jenkins
       tasks:
        - debug:
            msg: "Warning: This will modify ALL Jenkins servers."
       roles:
        - common
        - docker

[45]

Wait For
^^^^^^^^

A condition can be searched for before continuing on to the next task.

Syntax:

.. code-block:: yaml

    wait_for:

Example:

.. code-block:: yaml

    wait_for:
      timeout: 60
    delegate_to: localhost

Common options:

-  delay = How long to wait (in seconds) before running the wait\_for
   check.
-  path = A file to check.
-  host = A host to check a connection to.
-  port = A port to check on the specified host.
-  connect\_timeout = How long to wait (in seconds) before retrying the
   connection.
-  search\_regex = A regular expression string to match from either a
   port or file.
-  state

   -  started = Check for a open port.
   -  stopped = Check for a closed port.
   -  drained = Check for active connections to the port.
   -  present = Check for a file.
   -  absent = Verify a file does not exist.

-  timeout = How long to wait (in seconds) before continuing on.

[45]

When
^^^^

The "when" function can be used to specify that a sub-task should only
run if the condition returns turn. This is similar to an "if" statement
in programming languages. It is usually the last line to a sub-task. [11]

"When" Example:

.. code-block:: yaml

     - package: name=httpd state=latest
        when: ansible_os_family == "CentOS"

"Or" example:

.. code-block:: yaml

    when: ansible_os_family == "CentOS" or when: ansible_os_family == "Debian"

"And" example:

.. code-block:: yaml

    when: (ansible_os_family == "Fedora") and
          (ansible_distribution_major_version == "26")

Errors
^^^^^^

These modules handle Playbook errors.

Any Errors Fatal
''''''''''''''''

By default, a Playbook will continue to run on all of the hosts that do
not have any failures reported by modules. It is possible to stop the
Playbook from running on all hosts once an error has occurred. [12]

Syntax:

.. code-block:: yaml

    any_errors_fatal: true

Example:

.. code-block:: yaml

    - hosts: nfs_servers
      any_errors_fatal: true
      roles:
       - nfs

Fail
''''

The simple ``fail`` module will make a Playbook fail. This is useful
when checking if a certain condition has to exist to continue on.

All options:

-  msg = An optional message to provide the end-user.

Syntax:

.. code-block:: yaml

    fail:

Example:

.. code-block:: yaml

    - fail:
        msg: "Unexpected return code."
      when: (command_variable.rc != 0) or (command_variable.rc != 900)

[45]

Failed When
'''''''''''

In some situations, a error from a command or module may not be reported
properly. This module can be used to force a failure based on a certain
condition. [12]

Syntax:

.. code-block:: yaml

    failed_when: <CONDITION>

Example:

.. code-block:: yaml

    - command: echo "Testing a failure. 123."
      register: cmd
      failed_when: "'123' in cmd.stdout"

Ignore Errors
'''''''''''''

Playbooks, by default, will stop running on a host if it fails to run a
module. Sometimes a module will report a false-positive or an error will
be expected. This will allow the Playbook to continue onto the next
step. [12]

Syntax:

.. code-block:: yaml

    ignore_errors: yes

Example:

.. code-block:: sh

    - name: Even though this will fail, the Playbook will keep running.
      package: name=does-not-exist state=present
      ignore_errors: yes

Includes
^^^^^^^^

Include and import modules allow other elements of a Playbook to be
called and executed.

Import Playbook
'''''''''''''''

The proper way to use other Playbooks in a Playbook is to use the
``import_playbook``. Before Ansible 2.4 this was handled via the
``include`` module. There is also no ``include_playbook`` module, only
``import_playbook``.

Syntax:

.. code-block:: yaml

    ---
    - import_playbook: <PLAYBOOK>

Example:

.. code-block:: yaml

    ---
    - import_playbook: nginx.yml
    - import_playbook: phpfpm.yml
    - import_playbook: mariadb.yml

[49][58]

Import and Include Role
'''''''''''''''''''''''

The ``import_role`` is a static inclusion of a role that cannot be used
in loops. This is loaded on runtime of the Playbook

The ``include_role`` is a dynamic inclusion of a role that can be used
in loops. Tags will not automatically be shown with the ``--list-tags``
Ansible Playbook argument. This can be loaded dynamically based on
conditions. [49][58]

All options:

-  allow\_duplicates = Allow a role to be used more than once. Default:
   True.
-  defaults\_from = A default variable file to load from the role's
   "default" directory.
-  **name** = The name of the role to import.
-  private = All of the "default" an "vars" variables in the role are
   private and not accessible via the rest of the Playbook.
-  tasks\_from = A task file to load from the role's "tasks" directory.
-  vars\_from = A variables file to load from the role's "vars"
   directory.

Syntax:

.. code-block:: yaml

    - import_role: <ROLE_NAME>

.. code-block:: yaml

    - include_role: <ROLE_NAME>

Examples:

.. code-block:: yaml

    - name: Run only the install.yml task from the openshift role
      import_role:
        name: openshift
        tasks_from: install

.. code-block:: yaml

    - name: Run the Nagios role
      include_role:
        name: nagios
      vars:
        listen_port: 8080

[45]

Import and Include Tasks
''''''''''''''''''''''''

Use the ``import_tasks`` to statically include tasks at a Playbook's
runtime or ``include_tasks`` to dynamically run tasks once the Playbook
gets to it.

Syntax:

.. code-block:: yaml

    - import_tasks: <TASK_FILE>.yml

.. code-block:: yaml

    - include_tasks: <TASK_FILE>.yml

[49]

Include
'''''''

**Deprecated in: 2.4 Replaced by: include\_tasks, import\_plays,
import\_tasks**

Other task files and Playbooks can be included. The functions in them
will immediately run. Variables can be defined for the inclusion as
well. [49]

Syntax:

.. code-block:: yaml

    include:

.. code-block:: yaml

    include: <TASK>.yml <VAR1>=<VAULE1> <VAR2>=<VALUE2>

Example:

.. code-block:: yaml

    include: wine.yml wine_version=1.8.0 compression_format=xz download_util=wget

[45]

Include Variables
'''''''''''''''''

Additional variables can be defined within a Playbook file. These can be
openly added to the ``include_vars`` module via YAML syntax.

Common options:

-  file = Specify a filename to source variables from.
-  name = Store variables from a file into a specified variable.

Syntax:

.. code-block:: yaml

    include_vars: <VARIABLE>

Examples:

.. code-block:: yaml

    - hosts: all
      include_vars:
       - gateway: "192.168.0.1"
       - netmask: "255.255.255.0"
      roles:
       - addressing

.. code-block:: yaml

    - hosts: all
      include_vars: file=monitor_vars.yml
      roles:
       - nagios

[45]

Loops (Ansible < 2.5)
^^^^^^^^^^^^^^^^^^^^^

Loops can be used to iterate through lists and/or dictionaries. The most
commonly used loop is "with\_items."

Valid loops:

-  `Until <http://docs.ansible.com/ansible/2.4/playbooks_loops.html#do-until-loops>`__
-  `WithDict[ionary] <http://docs.ansible.com/ansible/2.4/playbooks_loops.html#looping-over-hashes>`__
-  `With First
   Found <http://docs.ansible.com/ansible/2.4/playbooks_loops.html#finding-first-matched-files>`__
-  `With
   Flattened <http://docs.ansible.com/ansible/2.4/playbooks_loops.html#flattening-a-list>`__
-  `With
   File <http://docs.ansible.com/ansible/2.4/playbooks_loops.html#looping-over-files>`__
-  `With
   Fileglob <http://docs.ansible.com/ansible/2.4/playbooks_loops.html#id4>`__
-  `With
   Filetree <http://docs.ansible.com/ansible/2.4/playbooks_loops.html#looping-over-filetrees>`__
-  `With Indexed
   Items <http://docs.ansible.com/ansible/2.4/playbooks_loops.html#looping-over-a-list-with-an-index>`__
-  `With
   INI <http://docs.ansible.com/ansible/2.4/playbooks_loops.html#using-ini-file-with-a-loop>`__
-  `With Inventory
   Hostnames <http://docs.ansible.com/ansible/2.4/playbooks_loops.html#looping-over-the-inventory>`__
-  `With
   Items <http://docs.ansible.com/ansible/2.4/playbooks_loops.html#standard-loops>`__
-  `With
   Lines <http://docs.ansible.com/ansible/2.4/playbooks_loops.html#iterating-over-the-results-of-a-program-execution>`__
-  `With
   Nested <http://docs.ansible.com/ansible/2.4/playbooks_loops.html#nested-loops>`__
-  `With Random
   Choice <http://docs.ansible.com/ansible/2.4/playbooks_loops.html#random-choices>`__
-  `With
   Sequence <http://docs.ansible.com/ansible/2.4/playbooks_loops.html#looping-over-integer-sequences>`__
-  `With
   Subelements <http://docs.ansible.com/ansible/2.4/playbooks_loops.html#looping-over-subelements>`__
-  `With
   Together <http://docs.ansible.com/ansible/2.4/playbooks_loops.html#looping-over-parallel-sets-of-data>`__


With First Found
''''''''''''''''

Multiple file locations can be checked to see what file exists. The
first file found in a given list will be returned to the task. [[10]

Syntax:

.. code-block:: yaml

    with_first_round:
      - <FILE1>
      - <FILE2>
      - <FILE3>

Example:

.. code-block:: yaml

    - name: Copy over the first Nova configuration that is found
      copy: src={{ item }} dest=/etc/nova/ remote_src=true
      with_first_found:
       - "/root/nova.conf"
       - "/etc/nova_backup/nova.conf"

With Flattened
''''''''''''''

Lists and dictionaries can be converted into one long string. This
allows a task to run once with all of the arguments. This is especially
useful for installing multiple packages at once. [[10]

Loop syntax:

.. code-block:: yaml

    with_flattened:
       - <LIST_OR_DICT>
       - <LIST_OR_DICT>

Variable syntax:

::

    {{ item }}

Example:

.. code-block:: yaml

    - set_fact: openstack_client_packages="[ 'python2-cinderclient', 'python2-glanceclient', python2-keystoneclient', 'python2-novaclient', 'python2-neutronclient' ]"

    - service: name={{ item }} state=restarted
      with_flattened:
       - "{{ openstack_client_packages }}"
       - python2-heatclient
       - [ 'python2-manilaclient', 'python2-troveclient' ]

With Items
''''''''''

A task can be re-used with items in a list and/or dictionary. [[10]

Loop syntax:

.. code-block:: yaml

    with_items:
      - <ITEM1>
      - <ITEM2>
      - <ITEM3>

List variable syntax:

::

    {{ item }}

Dictionary variable syntax:

::

    {{ item.<INDEX_STARTING_AT_0> }}

::

    {{ item.<KEY> }}

List example:

.. code-block:: yaml

    - service: name={{ item }} state=started enabled=true
      with_items:
       - nginx
       - php-fpm
       - mysql

Dictionary example:

.. code-block:: yaml

    - user: name={{ item.name }} group={{ item.group }} password={{ item.2 }} state=present
      with_items:
       - { name: "bob", group: "colab", passwd: "123456" }
       - { name: "sam", group: "colab", passwd: "654321" }

Variables
^^^^^^^^^

These are modules relating to defining new variables.

Prompts
'''''''

Prompts can be used to assign a user's input as a variable. [9] Note
that this module is not compatible with Ansible Tower and that a Survey
should be created within Tower instead. [40]

Common options:

-  confirm = Prompt the user twice and then verify that the input is the
   same.
-  encrypt = Encrypt the text.

   -  md5\_crypt
   -  sha256\_crypt
   -  sha512\_crypt

-  salt = Specify a string to use as a salt for encrypting.
-  salt\_size = Specify the length to use for a randomly generated salt.
   The default is 8.

Syntax:

.. code-block:: yaml

    vars_prompt:
      - name: "<VARIABLE>"
        prompt: "<PROMPT TEXT>"

Examples:

.. code-block:: yaml

    vars_prompt:
      - name: "zipcode"
        prompt: "Enter your zipcode."

.. code-block:: yaml

    vars_prompt:
       - name: "pw"
         prompt: "Password:"
         encrypt: "sha512_crypt"
         salt_size: 12

[9]

Register
''''''''

The output of modules and commands can be saved to a variable.

Variable return values [32]:

-  backup\_file = String. If a module creates a backup file, this is
   that file's name.
-  changed = Boolean. If something was changed after the module runs,
   this would be set to "true."
-  failed = Boolean. Shows if the module failed.
-  invocation = Dictionary. This describes the module used to run the
   operation as well as all of the arguments.
-  msg = String. A message that is optionally given to the end-user.
-  rc = Integer. The return code of a command, shell, or similar module.
-  stderr = String. The standard error of the command.
-  stderr\_lines = List. The standard output of the command is separated
   by the newline characters into a list.
-  stdout = String. The standard output of the command.
-  stdout\_lines = List.
-  results = List of dictionaries. If a loop was used, the results for
   each loop are stored as a new list item.
-  skipped = Boolean. If this module was skipped or not.

Syntax:

.. code-block:: yaml

    register: <NEW_VARIABLE>

Examples:

.. code-block:: yaml

     - command: echo Hello World
        register: hello
     - debug: msg="We heard you"
        when: "'Hello World' in hello.stdout"

.. code-block:: yaml

    - copy: src=example.conf dest=/etc/example.conf
      register: copy_example
    - debug: msg="Copying example.conf failed."
      when: copy_example|failed

[12]

Set Fact
''''''''

New variables can be defined set the "set\_fact" module. These are added
to the available variables/facts tied to a inventory host. [45]

Syntax:

.. code-block:: yaml

    set_fact:
      <VARIABLE_NAME1>: <VARIABLE_VALUE1>
      <VARIABLE_NAME2>: <VARIABLE_VALUE2>

Example:

.. code-block:: yaml

    - set_fact:
        is_installed: True
        sql_server: mariadb

UNIX Modules
~~~~~~~~~~~~

Command and Shell
^^^^^^^^^^^^^^^^^

Both the command and shell modules provide the ability to run command
line programs. The big difference is that shell provides a full shell
environment where operand redirection and pipping works, along with
loading up all of the shell variables. Conversely, command will not load
a full shell environment so it will lack in features and functionality
but it makes up for that by being faster and more efficient. [6]

Syntax:

.. code-block:: yaml

    command:

.. code-block:: yaml

    shell:

Common options:

-  executable = Set the executable shell binary.
-  chdir = Change directories before running the command.

Example:

.. code-block:: yaml

    - shell: echo "Hello world" >> /tmp/hello_world.txt
      args:
        executable: /bin/bash

Copy, File, Synchronize, and Template
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``copy``, ``file``, ``synchronize``, and ``template`` modules
provide ways for creating and modifying various files. The ``file``
module is used to handle file creation/modification on the remote host.
``template``\ s are to be used when a file contains variables that will
be rendered out by Jinja2. ``copy`` is used for copying files and
folders either from the role or on the remote host. ``synchronize`` is
used as a wrapper around rsync to provide a more robust copy
functionality. Most of the options and usage are the same between these
four modules.

Syntax:

.. code-block:: yaml

    copy:

.. code-block:: yaml

    file:

.. code-block:: yaml

    synchronize:

.. code-block:: yaml

    template:

Common options:

-  src = Define the source file or template. If a full path is not
   given, Ansible will check in the roles/\ ``<ROLENAME>``/files/
   directory for a file or roles/\ ``<ROLENAME>``/templates/ for a
   template. If the src path ends with a "/" then only the files within
   that directory will be copied (not the directory itself).
-  dest (or path) = This is the full path to where the file should be
   copied to on the destination server.
-  owner = Set the user owner.
-  group = Set the group owner.
-  setype = Set SELinux file permissions.

Copy, file, and template options:

-  mode = Set the octal or symbolic permissions. If using octal, it has
   to be four digits. The first digit is generally the flag "0" to
   indicate no special permissions.

Copy options:

remote\_src = If set to ``true``, the source file will be found on the
server Ansible is running tasks on (not the local machine). The default
is ``false``.

File options:

-  state = Specify the state the file should be created in.

   -  file = Copy the file.
   -  link = Create a soft link shortcut.
   -  hard = Create a hard link reference.
   -  touch = Create an empty file.
   -  directory = Create all subdirectories in the destination folder.
   -  absent = Delete destination folders.

Synchronize options:

-  archive = Preserve all of the original file permissions. The default
   is ``yes``.
-  delete = Remove files in the destination directory that do not exist
   in the source directory.
-  mode

   -  push = Default. Copy files from the source to the destination
      directory.
   -  pull = Copy files from the destination to the source directory.

-  recursive = Recursively copy contents of all sub-directories. The
   default is ``no``.
-  rsync\_opts = Provide additional ``rsync`` command line arguments.

Example:

-  Copy a template from roles/\ ``<ROLE>``/templates/ and set the
   permissions for the file.

.. code-block:: yaml

    template: src=example.conf.j2 dst=/etc/example/example.conf mode=0644 owner=root group=nobody

[46]

Cron
^^^^

The cron module is used to manage crontab entries. Crons are
scheduled/automated tasks that run on Unix-like systems.

Syntax:

.. code-block:: yaml

    cron:

Common options:

-  user = Modify the specified user's crontab.
-  job = Provide a command to run when the cron reaches the correct
-  minute
-  hour
-  weeekday = Specify the weekday as a number 0 through 6 where 0 is
   Sunday and 6 is Saturday.
-  month
-  day = Specify the day number in the 30 day month.
-  backup = Backup the existing crontab. The "backup\_file" variable
   provides the backed up file name.

   -  yes
   -  no

-  state

   -  present = add the crontab
   -  absent = remove an existing entry

-  special\_time

   -  reboot
   -  yearly or annually
   -  monthly
   -  weekly
   -  daily
   -  hourly

Example #1:

.. code-block:: yaml

    cron: job="/usr/bin/wall This actually works" minute="*/1" user=ubuntu

Example #2:

.. code-block:: yaml

    cron: job="/usr/bin/yum -y update" weekday=0 hour=6 backup=yes

[55]

Expect
^^^^^^

The ``expect`` module executes a command, searches for a regular
expression pattern and, if found, it will provide standard input back to
the command.

All options:

-  chdir = Change into a different directory before running the command.
-  **command** = The command to execute.
-  creates = A path to a file which should be created after the command
   executes properly.
-  echo = Show the response strings that were used.
-  removes = A path to a file which should not exist after the command
   executes properly.
-  **responses** = A dictionary of patterns to search for and responses
   that they should provide back.
-  timeout = The time, in seconds, to wait for finding the pattern.

Syntax:

.. code-block:: yaml

    expect:
      command: <COMMAND>
      responses:
        <PATTERN>: <RESPONSE_TO_USE>

Example:

.. code-block:: yaml

    - name: Find all of the available fruit
      expect:
        command: mysql -u dave -p -e 'SELECT fruit_name FROM food.fruits;'
        responses:
          password: "{{ mysql_pass_dave }}"

[6]

Get URL
^^^^^^^

The ``get_url`` module is used to download files from online.

Common options:

-  backup = Backup the destination file if it already exists. Default:
   no.
-  checksum = Specify a checksum method to use and the hash that is
   expected.
-  **dest** = Where the downloaded file should be saved to
-  timeout = The time, in seconds, to wait for a connection to the URL
   before failing. Default: 10.
-  {group\|mode\|owner} = Specify the permissions for the downloaded
   file.
-  **url** = The URL to download.
-

   -  use\_proxy = Use the proxy settings from the environment
      variables. Default: yes.

-  validate\_certs = Validate SSL certificates. Default: yes.

Syntax:

.. code-block:: yaml

    get_url:

Example:

.. code-block:: yaml

    - name: Downloading a configuration file
      get_url:
        url: https://internal.domain.tld/configs/nginx/nginx.conf
        dest: /etc/nginx/nginx.conf
        owner: nginx
        group: nginx
        mode: 0644
        validate_certs: no

[54]

Git
^^^

Git is a utility used for provisioning and versioning software. Ansible
has built-in support for handling most Git-related tasks.

Syntax:

.. code-block:: yaml

    git:

Common options:

-  repo = The full path of the repository.
-  dest = The path to place/use the repository
-  update = Pull the latest version from the Git server. The default is
   "yes."
-  version = Switch to a different branch or tag.
-  ssh\_opts = If using SSH, specify custom SSH options.
-  force = Override local changes. The default is "yes."

[7]

Service
^^^^^^^

The service module is used to handle system services.

Syntax:

.. code-block:: yaml

    service:

Common options:

-  name = Specify the service name.
-  enabled = Enable the service to start on boot or not. Valid options
   are "yes" or "no."
-  sleep = When restarted a service, specify the amount of time (in
   seconds) to wait before starting a service after stopping it.
-  state = Specify what state the service should be in.
-  started = Start the service.
-  stopped = Stop the service.
-  restarted = Stop and then start the service.
-  reloaded = If supported by the service, it will reload it's
   configuration file without restarting it's main thread. [55]

Example:

-  Restart the Apache service "httpd."

   .. code-block:: yaml

       service: name=httpd state=restarted sleep=3

MySQL Database and User
^^^^^^^^^^^^^^^^^^^^^^^

MySQL databases and users can be managed via Ansible. It requires the
"MySQLdb" Python library and the "mysql" and "mysqldump" binaries.

MySQL database syntax:

.. code-block:: yaml

    mysql_db:

MySQL user syntax:

.. code-block:: yaml

    mysql_user:

Options:

-  name = Specify the database name. The word "all" can be used to
   control all databases.
-  state
-  present = Create the database.
-  absent = Delete the database.
-  dump = Backup the database.
-  import = Import a database.
-  target = Specify a dump or import location.
-  config\_file = Specify the user configuration file. Default:
   "~/.my.cnf." Alternatively, login credentials can be manually
   specified.
-  login\_host = The MySQL server's IP or hostname. Default:
   "localhost."
-  login\_user = The MySQL username to login as.
-  login\_password = The MySQL user's password.
-  login\_port = The MySQL port to connect to. Default: "3306."
-  login\_unix\_socket = On Unix, a socket file can be used to connect
   to MySQL instead of a host and port.
-  connection\_timeout = How long to wait (in seconds) before closing
   the MySQL connection. The default is "30." [16]
-  priv (mysql\_user) = The privileges for the MySQL user. [17]

Example #1:

.. code-block:: yaml

    mysql_db: name=toorsdb state=present config_file=/secrets/.my.cnf

Example #2:

.. code-block:: yaml

    mysql_user: name=toor login_user=root login_password=supersecret priv=somedb.*:ALL state=present

Example #3:

.. code-block:: yaml

    mysql_user: name=maxscale host="10.0.0.%" priv="*.*:REPLICATION CLIENT,SELECT" password=supersecure123 state=present

Stat
^^^^

This module provides detailed information about a file, directory, or
link. It was designed to be similar to the Unix command ``stat``. All
the information from this module can be saved to a variable and accessed
as a from new ``<VARIABLE>.stat`` dictionary.

Syntax:

.. code-block:: yaml

    stat: path=<FILE>
    register: <STAT_VARIABLE>

Example:

.. code-block:: yaml

    - stat: path=/root/.ssh/id_rsa
      register: id_rsa

    - file: path=/root/.ssh/id_rsa mode=0600 owner=root group=root
      when: id_rsa.stat.mode is not "0600"

Common options:

-  checksum\_algorithm = The algorithm to use for finding the checksum.

   -  sha1 (Default)
   -  sha224
   -  sha256
   -  sha384
   -  sha512

-  follow = Follow symbolic links.
-  get\_checksum = If the SHA checksum should be generated.
-  get\_md5 = Boolean. If the MD5 checksum should be generated.
-  path = Required. String. The full path to the file.

``stat`` dictionary values:

-  {a\|c\|m}time = Float. The last time the file was either accessed,
   the metadata was created, or modified.
-  attributes = List. All of the file attributes.
-  charset = String. The text file encoding format.
-  checksum = String. The has of the path.
-  dev = Integer. The device the inode exists on.
-  {executable\|readable\|writeable} = Boolean. If the file is
   executable, readable, or writeable by the remote user that Ansible is
   running as.
-  exists = Boolean. If the file exists or not.
-  {gr\|pw}\_name = String. The name of the group or user owner.
-  isblk = Boolean. If the file is a block device.
-  ischr = Boolean. If the file is a character device for standard input
   or output.
-  isdir = Boolean. If the file is a directory.
-  isfifo = Boolean. If the file is a named pipe.
-  islink = Boolean. If the file is a symbolic link.
-  inode = Integer. The Unix inode number of the file.
-  isreg = Boolean. If the file is a regular file.
-  issock. Boolean. If the file is a Unix socket.
-  is{uid\|gid} = Boolean. If the file is owned by the user or group
   that the remote Ansible user is running as.
-  lnk\_source = String. The original path of the symbolic link.
-  md5 = String. The MD5 hash of the file.
-  mime\_type = The "magic data" that specifies the file type.
-  mode = Octal Unix file permissions.
-  nlink. Integer. The number of links that are used to redirect to the
   original inode.
-  path = String. The full path to the file.
-  {r\|w\|x}usr = Boolean. If the user owner has readable, writeable, or
   executable permissions.
-  {r\|w\|x}grp = Boolean. If the group owner has readable, writeable,
   or executable permissions.
-  {r\|w\|x}oth = Boolean. If other users have readable, writeable, or
   executable permissions.
-  size = Integer. The size, in bytes, of the file.
-  {uid\|gid} = Integer. The ID of user or group owner of the file.

[46]

URI
^^^

The ``uri`` module is used for handling HTTP requests.

Common options:

-  HEADER\_\* = Modify different types of header content.
-  body = The body of the request to send.
-  body\_format = The format to uses for the body. Default: raw.

   -  json
   -  raw

-  dest = A path to where a file should be downloaded to.
-  follow\_redirects = Default: safe.

   -  all = Follo wall redirects.
   -  none = Do not follow any redirects.
   -  safe = Follow the first redirect only.

-  method = The HTTP method type to use. Default: GET.

   -  CONNECT
   -  DELETE
   -  GET
   -  HEAD
   -  OPTIONS
   -  PATCH
   -  POST
   -  PUT
   -  REFRESH
   -  TRACE

-  password = The password to use for basic HTTP authentication.
-  status\_code = The expected status code from the request. Default:
   200.
-  timeout = When a connection to a URL should time out if it's
   unreachable.
-  **url** = The HTTP URL to connect to.
-  user = The username to use for basic HTTP authentication.

Syntax:

.. code-block:: yaml

    uri:

Example:

.. code-block:: yaml

    - name: Authenticate with OpenStack's Keystone v3 service
      uri:
        HEADER_Content-Type="application/json"
        body_format: json
        body: >
    {
        "auth": {
            "identity": {
                "methods": [
                    "password"
                ],
                "password": {
                    "user": {
                        "domain": {
                            "name": "Default"
                        },
                        "name": "admin",
                        "password": "{{ admin_pass }}"
                    }
                }
            },
            "scope": {
                "project": {
                    "domain": {
                        "name": "Default"
                    },
                    "name": "demo"
                }
            }
        }
    }
        method: POST
        url: https://openstack.tld:5000/v3/auth/tokens
      register: os_token_request

[54]

Package Managers
^^^^^^^^^^^^^^^^

Ansible has the ability to add, remove, or update software packages.
Almost every popular package manager is supported. This can
generically be handled by the "package" module or the specific module
for the operating system's package manager.

Syntax:

.. code-block:: yaml

    package:

Common options:

-  name = Specify the package name.
-  state = Specify how to change the package state.
-  present = Install the package.
-  latest = Update the package (or install, if necessary).
-  absent = Uninstall the package.
-  use = Specify the package manager to use.
-  auto = Automatically detect the package manager to use. This is the
   default.
-  apt = Use Debian's Apt package manager.
-  yum = Use Red Hat's yum package manager.

Example:

-  Update the MariaDB package.

.. code-block:: yaml

    package: name=mariadb state=latest

[47]

Apt
'''

Apt is used to install and manage packages on Debian based operating
systems.

Common options:

-  name = The package name.
-  state

   -  present = Install the package.
   -  latest = Update the package.
   -  absent = Uninstall the package.
   -  build-dep = Install the build dependencies for the source code.

-  update\_cache = Update the Apt cache (apt-get update). Default: no.
-  deb = Install a specified \*.deb file.
-  autoremove = Remove all dependencies that are no longer required.
-  purge = Delete configuration files.
-  install\_recommends = Install recommended packages.
-  upgrade

   -  no = Do not upgrade any system packages (default).
   -  yes = Update all of the system packages (apt-get upgrade).
   -  full = Update all of the system packages and uninstall older,
      conflicting packages (apt-get full-upgrade).
   -  dist = Upgrade the operating system (apt-get dist-upgrade).

[47]

Yum
'''

There are two commands to primarily handle Red Hat's Yum package
manager: "yum" and "yum\_repository."

Syntax:

.. code-block:: yaml

    yum:

Common options:

-  name = Specify the package name.
-  state = Specify the package state.
-  {present\|installed\|latest} = Any of these will install the package.
-  {absent\|removed} = Any of these will uninstall the package.
-  enablerepo = Temporarily enable a repository.
-  disablerepo = Temporarily disable a repository.
-  disable\_gpg\_check = Disable the GPG check. The default is "no".
-  conf\_file = Specify a Yum configuration file to use.

Example:

-  Install the "wget" package with the EPEL repository enabled and
   disable GPG validation checks.

.. code-block:: yaml

    yum: name=wget state=installed enablerepo=epel disable_gpg_check=yes

Yum repository syntax:

.. code-block:: yaml

    yum_repository:

Common options:

-  baseurl = Provide the URL of the repository.
-  **description** = Required if ``state=present``. Provide a
   description of the repository.
-  enabled = Enable the repository permanently to be active. The default
   is "yes."
-  exclude = List packages that should be excluded from being accessed
   from this repository.
-  gpgcheck = Validate the RPMs with a GPG check. The default is "no."
-  gpgkey = Specify a URL to the GPG key.
-  includepkgs = A space separated list of packages that can be used
   from this repository. This is an explicit allow list.
-  mirrorlist = Provide a URL to a mirrorlist repository instead of the
   baseurl.
-  **name** = Required. Specify a name for the repository. This is only
   required if the file is being created (state=present) or deleted
   (state=absent).
-  reposdir = The directory to store the Yum configuration files.
   Default: ``/etc/yum.repos.d/``.
-  state = Specify a state for the repository file.
-  present = Install the Yum repository file. This is the default.
-  absent = Delete the repository file.

Example:

-  Install the RepoForge Yum repository.

.. code-block:: yaml

    yum_repository: name=repoforge baseurl=http://apt.sw.be/redhat/el7/en/x86_64/rpmforge/ enabled=no description="Third-party RepoForge packages"

[47]

Windows Modules
~~~~~~~~~~~~~~~

These modules are specific to managing Windows servers and are not
related to the normal modules designed for UNIX-like operating systems.
These module names start with the "win\_" prefix.

Command and Shell
^^^^^^^^^^^^^^^^^

Windows commands can be executed via a console. The ``command`` module
uses the DOS "cmd" binary and shell, by default, uses PowerShell.

All similar ``command`` and ``shell`` options:

-  chdir = Change the current working directory on the remote server
   before executing a command.
-  creates = A path (optionally with a regular expression pattern) to a
   file. If this file already exists, this module will be marked as
   "skipped."
-  removes = If a path does not exist then this module will be marked as
   "skipped."

``shell`` options:

-  executable = The binary to use for executing commands. By default
   this is PowerShell. Use "cmd" for running DOS commands.

Syntax:

.. code-block:: yaml

    win_command:

.. code-block:: yaml

    win_shell

Example:

::

    win_shell: "echo Hello World > c:\hello.txt"
      chdir: "c:\"
      creates: "c:\hello.txt"

[48]

File Management
^^^^^^^^^^^^^^^

Copy
''''

Copy files from the Playbook to the remote server.

All options:

-  content = Instead of using ``src``, specify the text that should
   exist in the destination file.
-  **dest** = The destination to copy the file to.
-  force = Replace files in the destination path if there is a conflict.
   Default: True.
-  remote\_src = Copy a file from one location on the remote server to
   another on the same server.
-  **src** = The source file to copy.

Syntax:

.. code-block:: yaml

    win_copy:

Example:

.. code-block:: yaml

    - name: Copying a configuration file
      win_copy:
        src: C:\Windows\example.conf
        dest: C:\temp\
        remote_src: True

[48]

File
''''

All options:

-  **path** = The full path to the file on the remote server that should
   be created, removed, and/or checked.
-  state

   -  absent = Delete the file.
   -  directory = Create a directory.
   -  file = Check to see if a file exists. Do not create a file if it
      does not exist.
   -  touch = Create a file if it does not exist.

Syntax:

.. code-block:: yaml

    win_file:

Example:

.. code-block:: yaml

    - win_file:
        path: C:\Users\admin\runtime_files
        state: directory

[48]

Robocopy
''''''''

Robocopy is a CLI utility, available on the latest versions of Windows,
for synchronizing directories.

All options:

-  **dest** = The destination directory.
-  flags = Provide additional arguments to the robocopy command.
-  purge = Delete files in the destination that do not exist in the
   source directory.
-  recurse = Recursively copy subdirectories.
-  **src** = The source directory to copy from.

Syntax:

.. code-block:: yaml

    win_robocopy:

Example:

.. code-block:: yaml

    win_robocopy:
      src: C:\tmp\
      dest: C:\tmp_old\
      recurse: True

[48]

Shortcut
''''''''

Manage Windows shortcuts.

All options:

-  args = Arguments to provide to the source executable.
-  description = A description about the shortcut.
-  **dest** = The path and file name of the shortcut. For executables
   use the extension ``.lnk`` and for URLs use ``.url``.
-  directory = The work directory for the executable.
-  hotkey = The combination of keys to virtually press when the shortcut
   is executed.
-  icon = A ``.ico`` icon file to use as the shortcut image.
-  src = The executable or URL that the shortcut should open.
-  state

   -  absent = Delete the shortcut.
   -  present = Create the shortcut.

-  windowstyle = How the program's window is sized during launch.

   -  default
   -  maximized
   -  minimized

Syntax:

.. code-block:: yaml

    win_shortcut:

Example:

.. code-block:: yaml

    win_shortcut:
      src: C:\Program Files (x86)\game\game.exe
      dest: C:\Users\Ben\Desktop\game.lnk

[48]

Template
''''''''

The Windows Jinja2 template module uses the same options as the normal
``template`` module.

Syntax:

.. code-block:: yaml

    win_template:

[48]

Installations
^^^^^^^^^^^^^

Chocolatey
''''''''''

Chocolatey is an unofficial package manager for Windows. Packages can be
installed from a public or private Chocolatey repository.

Common options:

-  force = Reinstall an existing package.
-  install\_args = Arguments to pass to Chocolatey during installation.
-  ignore\_dependencies = Ignore dependencies of a package. Default: no.
-  **name** = The name of a package to manage.
-  source = The Chocolatey repository to use.
-  state = Default: present.

   -  absent = Uninstall the package.
   -  present = Install the package.
   -  latest = Update the package.

-  timeout = The number of seconds to wait for Chocolatey to complete
   it's action. Default: 2700.
-  version = The exact version of a package that should be installed.

Syntax:

.. code-block:: yaml

    win_chocolatey:

Example:

.. code-block:: yaml

    win_chocolatey: name="libreoffice" state="upgrade" version="5.4.0"

[48]

Feature
'''''''

Manage official features and roles in Windows.

All options:

-  include\_management\_tools = Install related management tools. This
   only works in Windows Server >= 2012.
-  include\_sub\_features = Install all subfeatures related to the main
   feature.
-  **name** = The name of the feature or role.
-  restart = Restart the server after installation.
-  source = The path to the local package of the feature. This only
   works in Windows Server >= 2012.
-  state

   -  absent = Uninstall the feature.
   -  present = Install the feature.

Syntax:

.. code-block:: yaml

    win_feature:

Example:

.. code-block:: yaml

    - name: Install the IIS HTTP web server
      win_feature:
        name: Web-Server
        state: present

[48]

On Windows, all of the available features can be found via PowerShell.

.. code-block:: sh

    > Get-WindowsFeature

If part of the name is known, a PowerShell wildcard can be used to
narrow it down.

.. code-block:: sh

    > Get-WindowsFeature -Name <PART_OF_A_NAME>*

[39]

MSI
'''

**Deprecated in: 2.3 Replaced by: ``win_package``**

The MSI module is used to install executable packages. [48]

Package
'''''''

Manage official Microsoft packages for Windows. Examples of these
include the .NET Framework, Remote Desktop Connection Manager, Visual
C++ Redistributable, and more.

All options:

-  arguments = Arguments will be passed to the package during
   installation.
-  expected\_return\_code = The return code number that is expected
   after the installation is complete. Default: 0.
-  name = Optionally provide a friendly name for the package for Ansible
   logging purposes.
-  **path** = The file path or HTTP URL to a package.
-  **product\_id** = For verifying installation, the product ID is
   required to lookup in the registry if it is installed already.

   -  Note: This can be found at:

      -  64-bit:
         ``HKLM:Software\Microsoft\Windows\CurrentVersion\Uninstall``
      -  32-bit:
         ``HKLM:Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall``

-  state

   -  absent = Uninstall the package.
   -  present = Install the package.

-  user\_{name\|password} = Specify the username and password to access
   a SMB/CIFS share that contains the package.

Syntax:

.. code-block:: yaml

    win_package:

Example:

.. code-block:: yaml

    - name: 'Microsoft .NET Framework 4.5.1'
      win_package:
        path: https://download.microsoft.com/download/1/6/7/167F0D79-9317-48AE-AEDB-17120579F8E2/NDP451-KB2858728-x86-x64-AllOS-ENU.exe
        productid: '{7DEBE4EB-6B40-3766-BB35-5CBBC385DA37}'
        arguments: '/q /norestart'
        ensure: present
        # Return code "3010" means that Windows requires a reboot
        expected_return_code: 3010

[48]

Updates
'''''''

Windows Updates can be managed by Ansible.

All options:

-  category\_names = A list of categories to manage updates for. Valid
   categories are:

   -  Application
   -  Connectors
   -  CriticalUpdates (default)
   -  DefinitionUpdates
   -  DeveloperKits
   -  FeaturePacks
   -  Guidance
   -  SecurityUpdates (default)
   -  ServicePacks
   -  Tools
   -  UpdateRollups (default)
   -  Updates

-  log\_path = The path to a custom log file.
-  state

   -  installed = Search for and install updates.
   -  searched = Only search for updates.

Syntax:

.. code-block:: yaml

    win_updates:

Example:

.. code-block:: yaml

    win_updates: category_names=['CriticalUpdates'] state=searched log_path="c:\tmp\win_updates_log.txt"

[48]

Registry
^^^^^^^^

The registry can be viewed and edited using the
`win\_regedit <http://docs.ansible.com/ansible/latest/win_regedit_module.html>`__
and
`win\_reg\_stat <http://docs.ansible.com/ansible/latest/win_reg_stat_module.html>`__
modules.

Scheduled Task
^^^^^^^^^^^^^^

Manage scheduled tasks in Windows.

All options:

-  arguments = Arguments that should be supplied to the executable.
-  days\_of\_week = A list of weekdays to run the task.
-  description = A uesful description for the purpose of the task.
-  enabled = Set the task to be enabled or not.
-  executable = The command the task should run.
-  frequency = The frequency to run the command.

   -  once
   -  daily
   -  weekly

-  **name** = The name of the task.
-  path = The folder to store the task in.
-  **state**

   -  absent = Delete the task.
   -  present = Create the task.

-  time = The time to run the task.
-  user = The user to run the task as.

Syntax:

.. code-block:: yaml

    win_scheduled_task:

Example:

.. code-block:: yaml

    win_scheduled_task:
      name: RestartIIS
      executable: iisreset
      arguments: /restart
      days_of_week: saturday
      time: 2am

[48]

Service
^^^^^^^

Manage services on Windows.

All options:

-  dependencies = A list of other services that are dependencies for
   this service.
-  dependency\_action

   -  add = Append these dependencies to the existing dependencies.
   -  set = Set this list of dependencies as the only dependencies.
   -  remove = Remove these dependencies from the service.

-  description = A useful description of the service.
-  desktop\_interact = Allow the LocalSystem user to interact with the
   Windows desktop.
-  display\_name = A user-friendly name for the service.
-  force\_dependent\_services = Changing the state of this service will
   change the state of all of the dependencies.
-  **name** = The actual name of the service.
-  password = The password to authenticate with. For the LocalService,
   LocalSystem, and NetworkService users, the password has to be an
   empty string and not undefined.
-  path = The path to the executable for the service.
-  start\_mode

   -  auto = Automatically start on boot.
   -  delayed = Automatically start on boot after all of the "auto"
      services have started.
   -  disabled = Do not allow this service to be run.
   -  manual = The administrator has to manually start this task.

-  state

   -  absent = Delete the service.
   -  restarted = Restart the service.
   -  started = Start the service.
   -  stopped = Stop the service.

-  username = The user to run the service as.

Syntax:

.. code-block:: yaml

    win_service:

Example:

.. code-block:: yaml

    win_service:
      name: CustomService
      path: C:\Program Files (x86)\myapp\myapp.exe
      start_mode: auto
      username: .\Administrator
      password: {{ admin_pass }}

[48]

User
^^^^

Create, read, update, or delete (CRUD) a Windows user account.

All options:

-  account\_disabled = Disable the account. The user can no longer be
   used.
-  account\_locked = Lock the account. The user will no longer have
   access to log into their account.
-  description = A description of the user's purpose.
-  fullname = The full name of the user.
-  groups = A list of groups that the user should be added to or removed
   from.
-  groups\_actions

   -  replace = Add the user to each of the ``groups`` and remove them
      from all others.
   -  add = Add the user to each of the ``groups``.
   -  remove = Remove the user from all of the ``groups``.

-  **name** = The name of the user to modify.
-  password = The the user's password.
-  password\_expired = Force the user's password to be expired/changed.
-  password\_never\_expires = Determine if the user's password should
   ever expire.
-  state

   -  absent = Delete the user.
   -  present = Create the user. This is the default option.
   -  query = Look up information about the user account.

-  update\_password

   -  always = Change the password for a user.
   -  on\_create = Only change a password for a user that was just
      created.

-  user\_cannot\_change\_password = Allow or disallow a user from
   modifying their password.

Syntax:

.. code-block:: yaml

    win_user:

Example:

.. code-block:: yaml

    win_user: name="default" password="abc123xyz890" user_cannot_change_password="yes" groups=['privileged', 'shares'] state="present"

[48]

Module Development
~~~~~~~~~~~~~~~~~~

Official Ansible module development documentation:

-  http://docs.ansible.com/ansible/latest/dev\_guide/index.html

All of the helper libraries for Ansible can be found in
`lib/ansible/modules\_utils/ <https://github.com/ansible/ansible/tree/devel/lib/ansible/module_utils>`__.
At the bare minimum, the `AnsibleModule
class <https://github.com/ansible/ansible/blob/devel/lib/ansible/module_utils/basic.py>`__
should be used to create a new module object.

.. code-block:: python

    from ansible.module_utils.basic import AnsibleModule

That basic syntax and layout of creating a module object looks like
this.

.. code-block:: python

    module = AnsibleModule(
        argument_spec=dict(
            <ARGUMENT_NAME>=dict(<OPTIONS>)
        ),
        <OTHER_MODULE_OPTIONS>
        )

These are all of the various settings that can be defined and used
AnsibleModule object.

**``AnsibleModule`` initialization:**

-  argument\_spec = A dictionary of arguments that can be provided by a
   user using this module. Each argument can have it's own settings.

   -  ``<ARGUMENT_NAME>`` = A unique argument name should be given. This
      will contain a dictionary of additional settings for this
      argument.

      -  aliases = A list of other names that can be used to reference
         this same argument.
      -  choices = A list of explicit valid choices for this argument.
         This is primarily used for documentation.
      -  required = True or False. If this argument is required for the
         module to work.
      -  default = A default value to provide if the user does not
         specify one.
      -  type = The type of value that should be provided. This can be
         any valid Python variable type. Common types include:

         -  bool = Boolean.
         -  float = Float, a decimal number.
         -  int = Integer, a whole number.
         -  list
         -  path = A path to a file or directory.
         -  string

-  required\_one\_of = A list of arguments where at least one is
   required for the module to work.
-  mutually\_exclusive = A list of arguments that cannot be used
   together.
-  supports\_check\_mode = Specify if this module supports Ansible's
   "check mode" where it can check to see if this module will change
   anything without modifying the system. This sets the
   ``module.check_mode`` boolean.

**``module`` common object methods:**

-  \_deprecation = A dictionary of information for a deprecation
   message.

   -  msg = The deprecation string.
   -  version = The version this was / will be deprecated in.

-  \_warnings = A list of warnings to provide the end user.
-  append\_to\_file = Append text to a file.
-  atomic\_move = Copy a source file to a destination. The new
   destination file will use the same file attributes as the original
   destination file.
-  debug = Debug a variable's value.
-  digest\_from\_file = Return a checksum of a file.
-  exit\_json = A dictionary of return data when the module finishes
   successfully.

   -  *kwargs* = Any variables can be passed to this method and will be
      returned in the error message. Common variable names and values to
      pass include:

      -  changed = A boolean stating if anything has changed.
      -  changes = A dictionary of items that were changed.
      -  results = A dictionary of results that should be returned to
         the end user.

-  fail\_json = A dictionary for when the module fails.

   -  msg = A string of a failure message.
   -  *kwargs* = Any other variables can be passed to this method and
      will be returned in the error message.

-  from\_json = Convert JSON data into a dictionary.
-  get\_bin\_path = Find the path of a binary on the managed system.
-  jsonify = Convert a variable into JSON format.
-  run\_command = Run a command on the managed system. This method will
   return the return code, the standard output, and the standard error
   from the process. Example:

.. code-block:: python

    cmd = "echo Hello world"
    rc, out, err = module.run_command(cmd)

**``module`` common object variables:**

-  check\_mode = Boolean. Determines if check\_mode is supported based
   on what ``module.supports_check_mode`` value is set to.
-  params = Dictionary. All of the module argument variables.

[44]

Roles
-----

Roles are used to accomplish and/or manage one specific item. Usually
this will be to install and setup a program. A Playbook can be created
to use multiple roles.

Galaxy
~~~~~~

Ansible Galaxy provides a way to easily manage remote Ansible Galaxy
roles from https://galaxy.ansible.com/ and other software configuration
management (SCM) sources. [27]

.. code-block:: sh

    $ ansible-galaxy install <USER_NAME>.<ROLE_NAME>

.. code-block:: sh

    $ ansible-galaxy install <USER_NAME>.<ROLE_NAME>,<VERSION>

.. code-block:: sh

    $ ansible-galaxy install --roles-path <PATH> <USER_NAME>.<ROLE_NAME>

For a Role to work with Ansible Galaxy, it is required to have the
``meta/main.yml`` file. This will define supported Ansible versions and
systems, dependencies on other Roles, the license, and other useful
information. [58]

.. code-block:: yaml

    galaxy_info:
      author:
      description:
      company:
      license:
      min_ansible_version:
      platforms:
       - name: <OS_NAME>
      versions:
       - <OS_VERSION>
      categories:
      dependencies:
        - { role: <ROLE_NAME> }
        - { role: <ROLE_NAME>, <VARIABLE>: <VALUE> }

Dependencies
^^^^^^^^^^^^

Roles can define dependencies to other roles hosted remotely. By
default, the Ansible Galaxy repository is used to pull roles from.
Ansible Galaxy in itself uses GitHub.com as it's back-end. Dependencies
can be defined in ``requirements.yml`` or inside the role at
``meta/main.yml``.

Install the dependencies by running:

.. code-block:: sh

    $ ansible-galaxy install -r requirements.yml

-  Dependency options:

   -  src = The role to use. Valid formats are:

      -  ``<USER_NAME>.<ROLE_PROJECT_NAME>`` = The user and project name
         to use from GitHub.
      -  ``https://github.com/<USER>/<ROLE_PROJECT_NAME>``
      -  ``git+https://github.com/<USER>/<ROLE_PROJECT_NAME>.git`` =
         Explicitly use HTTPS for accessing GitHub.
      -  ``git+ssh://git@<DOMAIN>/<USER>/<ROLE_PROJECT_NAME>.git`` = Use
         SSH for accessing GitHub.

   -  version = The branch, tag, or commit to use. Default: master.
   -  name = Provide the role a new custom name.
   -  scm = The supply chain management (SCM) tool to use. Currently
      only Git (git) and Mercurial (hg) are supported. This is useful
      for using projects that are not hosted on GitHub.com. Default:
      git.

Dependency syntax:

.. code-block:: yaml

    dependencies:
      - src: <USER_NAME>.<ROLE_NAME>
        version: <VERSION>
        name: <NEW_ROLE_NAME>
        scm: <SCM>
      - src: <USER_NAME2>.<ROLE_NAME2>

Dependency example:

.. code-block:: yaml

    - src: https://github.com/hux/starkiller
      version: 3101u9e243r90adf0a98avn4bmz
      name: new_deathstar
    - src: https://example.tld/project
      scm: hg
      name: project

Git with SSH example (useful for GitLab):

.. code-block:: yaml

    - src: git+ssh://git@<DOMAIN>/<USER>/<PROJECT>.git
      version: 1.2.0
      scm: git

[27]

Community Roles
^^^^^^^^^^^^^^^

Unofficial community Roles can be used within Playbooks. Most of these
can be found on `Ansible Galaxy <https://galaxy.ansible.com/>`__ or
`GitHub <https://github.com/>`__. This section covers some useful Roles.

Network Interface
'''''''''''''''''

URL: https://github.com/MartinVerges/ansible.network\_interface

The ``network_interface`` role was created to help automate the
management of network interfaces on Debian and RHEL based systems. The
most up-to-date and currently maintained fork of the original project is
owned by the `GitHub user
MartinVerges <https://github.com/MartinVerges>`__.

The role can be passed any of these dictionaries to configure the
network devices.

-  network\_ether\_interfaces = Configure ethernet devices.
-  network\_bridge\_interfaces = Configure bridge devices.
-  network\_bond\_interfaces = Configure bond devices.
-  network\_vlan\_interfaces = Configure VLAN devices.

Valid dictionary values:

-  device = Required. This should define the device name to modify or
   create.
-  bootproto = Required. ``static`` or ``dhcp``.
-  address = Required for ``static``. IP address.
-  netmask = Required for ``static``. Subnet mask.
-  cidr = For ``static``. Optionally use CIDR notation to specify the IP
   address and subnet mask.
-  gateway = The default gateway for the IP address.
-  hwaddress = Use a custom MAC address.
-  mtu = Specify the MTU packet size.
-  vlan = Set to ``True`` for creating the VLAN devices.
-  bond\_ports = Required for bond interfaces. Specify the ethernet
   devices to use for the unified bond.
-  bond\_mode = For bond interfaces. Define the type of Linux bonding
   method.
-  bridge\_ports = Required for bridge interfaces. Specify the ethernet
   device(s) to use for the bridge.
-  dns-nameserver = A Python list of DNS nameservers to use.

Example:

-  ``eth0`` is configured to use DHCP and has it's MTU set to 9000.
-  ``eth1`` is added to the new bridge ``br0`` with the IP address
   ``10.0.0.1`` and the subnet mask of ``255.255.255.0``.
-  ``eth2`` and ``eth3`` are configured to be in a bond, operating in
   mode "6" (adaptive load balancing).
-  ``bond0.10`` and ``bond0.20`` are created as VLAN tagged devices off
   of the newly created bond.

.. code-block:: yaml

    - hosts: gluster01
      roles:
       - ansible.network_interfaces
         network_ether_interfaces:
          - device: eth0
            bootproto: dhcp
            mtu: 9000
         network_bridge_interfaces:
          - device: br0
            cidr: 10.0.0.1/24
            bridge_ports: [ "eth1" ]
         network_bond_interfaces:
          - device: bond0
            bootproto: static
            bond_mode: 6
            bond_ports: [ "eth2", "eth3" ]
         network_vlan_interfaces:
          - device: bond0.10
            vlan: True
            bootproto: static
          - device: bond0.20
            vlan: True
            bootproto: static

[30]

Jinja2
------

Jinja2 is the Python library used for variable manipulation and
substitution in Ansible. This is also commonly used when creating files
for the "``template``" module.

Variables
~~~~~~~~~

Variables defined in Ansible can be single variables, lists, and
dictionaries. This can be referenced from the template.

-  Syntax:

   ::

       {{ <VARIABLE> }}

   ::

       {{ <DICTIONARY>.<KEY> }}
       {{ <DICTIONARY>['<KEY>'] }}

-  Example:

   ::

       {{ certification.name }}

Variables can be defined as a list or nested lists.

Syntax:

::

    <VARIABLE>: [ '<ITEM1>', '<ITEM2>', '<ITEM3>' ]

::

    <VARIABLE>:
     - [ [ '<ITEMA>', '<ITEMB>' ] ]
     - [ [ '<ITEM1>', '<ITEM2>' ] ]

Examples:

::

    colors: [ 'blue', 'red', 'green' ]

::

    cars:
     - [ 'sports', 'sedan' ]
     - [ 'suv', 'pickup' ]

Lists can be called by their array position, starting at "0."
Alternatively they can be called by the subvariable name.

Syntax:

::

    {{ item.0 }}

::

    {{ item.0.<SUBVARIABLE> }}

Example:

.. code-block:: yaml

    members:
     - name: Frank
       contact:
        - address: "111 Puppet Drive"
        - phone: "1111111111"

.. code-block:: yaml

     - debug: msg="Contact {{ item.name }} at {{ item.contact.phone }}"
       with_items:
        - {{ members }}

[3]

Using a variable for a variable name is not possible with Jinja
templates. Only substitution for dictionary keys can be done with format
substitution.

Works:

.. code-block:: yaml

      - name: find interface facts
        debug:
          msg: "{{ hostvars[inventory_hostname]['ansible_%s' | format(item)] }}"
        with_items: "{{ ansible_interfaces }}"

Does not work:

.. code-block:: yaml

      - name: find interface facts
        debug:
          msg: "{{ ansible_%s| format(item)] }}"
        with_items: "{{ ansible_interfaces }}"

Filters
~~~~~~~

In certain situations it is desired to apply filters to alert a variable
or expression. The syntax for running Jinja filters is
``<VARIABLE>|<FUNCTION>(<OPTIONAL_PARAMETERS>)``. Below are some of the
more common functions.

-  Convert to a different variable type.

   ::

       {{ <VARIABLE>|string }}

   ::

       {{ <VARIABLE>|list }}

   ::

       {{ <VARIABLE>|int }}

   ::

       {{ <VARIABLE>|float }}

   ::

       {{ <VARIABLE>|bool }}

-  Convert a list into a string and optionally separate each item by a
   specified character.

   ::

       {{ <VARIABLE>|join("<CHARACTER>") }}

-  Create a default variable if the variable is undefined.

   ::

       {{ <VARIABLE>|default("<DEFAULT_VALUE>")

-  Convert all characters in a string to lower or upper case.

   ::

       {{ <VARIABLE>|lower }}

   ::

       {{ <VARIABLE>|upper }}

-  Round numbers.

   ::

       {{ <VARIABLE>|round }}

-  Escape HTML characters.

   ::

       {{ <VARIABLE>|escape }}

   ::

       {% autoescape true %}
       <html>These HTML tags will be
       escaped and visible via a HTML browser.</html>
       {% endautoescape %}

-  String substitution.

   ::

       {{ "%s %d"|format("I am this old:", 99) }}

-  Find the first or last value in a list.

   ::

       {{ <LIST>|first }}

   ::

       {{ <LIST>|last }}

-  Find the number of items in a variable.

   ::

       {{ <VARIABLE>|length }}

[20]

Comments
~~~~~~~~

Comments are template comments that will be removed when once a template
has been generated.

Syntax:

::

    {# #}

Example:

::

    {# this is a...
        {% if ip is '127.0.0.1' %}
            <html>Welcome to localhost</html>
        {% endif %}
    ...example comment #}

Sometimes it is necessary to escape blocks of code, especially when
dealing with JSON or other similar formats. Jinja will not render
anything that is escaped.

Syntax:

::

    {{ '' }}

::

    {% raw %}
    {% endraw %}

Examples:

::

      {{ 'hello world' }}

::

      {% raw %}
          {{ jinja.wont.replace.this }}
      {% endraw %}

[20]

Blocks
~~~~~~

Templates can extend other templates by replacing "block" elements. At
least two files are required. The first file creates a place holder
block. The second file contains the content that will fill in that place
holder.

Syntax (file 1):

::

    {% block <DESCRIPTIVE_NAME> %}{% endblock %}

Syntax (file 2):

::

    {% extends "<FILE1>" %}
    {% block <DESCRIPTIVE_NAME> %}
    {% endblock %}

Example (file 1):

.. code-block:: html

    <html>
    <h1>{% block header %}{% endblock %}</h1>
    <body>{% block body %}{% endblock %}</body>
    </html>

Example (file 2):

::

    {% extends "index.html" %}
    {% block header %}
    Hello World
    {% endblock %}
    {% block body %}
    Welcome to the Hello World page!
    {% endblock %}

[20]

Loops
~~~~~

Loops can use standard comparison and/or logic operators.

Comparison Operators:

-  ``==``
-  ``!=``
-  ``>``
-  ``>=``
-  ``<``
-  ``=<``

Logic Operators:

-  ``and``
-  ``or``
-  ``not``

"For" loops can be used to loop through a list or dictionary.

Syntax:

::

    {% for <VALUE> in {{ <LIST_VARIABLE> }} %}
    {% endfor %}

::

    {% for <KEY>, <VALUE> in {{ <DICTIONARY_VARIABLES> }} %}
    {% endfor %}

Examples:

::

    # /etc/hosts
    {% for host in groups['ceph'] %}
    hostvars[host]['private_ip'] hostvars[host]['ansible_hostname']
    {% endfor %}

::

    {% for count in range(1,4) %}
    [{{ groups['db'][{{ count }}] }}]
    type=server
    priority={{ count }}
    {% endfor %}

"For" loops have special variables that can be referenced relating to
the index that the loop is on.

-  loop.index = The current index of the loop, starting at 1.
-  loop.index0 = The current index of the loop, starting at 0.
-  loop.revindex = The same as "loop.index" but in reverse order.
-  loop.revindex = The same as "loop.index0" but in reverse order.
-  loop.first = This will be True if it is the first index.
-  loop.last = This will be True if is it the last index.

"If" statements can be run if a certain condition is met.

Syntax:

::

    {% if <VALUE> %}
    {% endif %}

::

    {% if <VALUE_1> %}
    {% elif <VALUE_2> %}
    {% else %}
    {% endif %}

Example:

::

    {% if {{ taco_day }} == "Tuesday" %}
        Taco day is on Tuesday.
    {% else %}
        Taco day is not on a Tuesday.
    {% endif %}

[20]

Python API
----------

Ansible is written in Python so it can be used programmatically to run Playbooks. This does not provide a thread-safe interface and is subject to change depending on the needs of the actual Ansible utilities. It is recommended to use a RESTful API from a dashboard such as the official AWX project. Using the direct Python libraries for Ansible is not recommended. [34]

Containers
----------

Ansible Container
~~~~~~~~~~~~~~~~~

The `official Ansible Container
project <https://docs.ansible.com/ansible-container/>`__ aims to allow
Playbooks to be deployed directly to docker containers. This allows
Ansible to orchestrate both infrastructure and applications.

Install Ansible Container into a Python virtual environment. This helps
to separate Python packages provided by the operating system's package
manager. Source the "activate" file to use the new Python environment.
[24]

.. code-block:: sh

    $ virtualenv ansible-container
    $ source ansible-container/bin/activate
    $ pip install -U pip setuptools
    $ pip install ansible-container[docker,openshift]

-  Ansible Container directory structure:

   -  container.yml = An Ansible file that mirrors Docker Compose syntax
      is used to define how to create the docker container. Common
      settings include the image to use, ports to open, commands to run,
      etc.
   -  ansible-requirements.txt = Python dependencies to install for
      Ansible.
   -  requirements.yml = Ansible dependencies to install from Ansible
      Galaxy.
   -  ansible.cfg = Ansible configuration for the container.

Example ``container.yml``:

.. code-block:: yaml

    version: "2"
    services:
      web:
        from: "centos:7"
        ports:
          - "80:80"
        command: ["/usr/bin/dumb-init", "/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
        dev_overrides:
          environment:
            - "DEBUG=1"

All of the Docker Compose options as specified at
https://docs.docker.com/compose/compose-file/.

Common ``container.yml`` options:

-  version = The version of Docker Compose to use. Valid options are
   ``1`` or ``2`` since Ansible Container 0.3.0.

   .. code-block:: yaml

       version: '2'

-  settings = Project configuration settings.

   -  conductor\_base = The container to run Ansible from. This should
      mirror the development environment used for Ansible Container.

Example:

      .. code-block:: yaml

          settings:
            conductor_base: centos:7

   -  deployment\_output\_path = The directory mounted for placing the
      generated Ansible Playbook(s). Default: ``./ansible-deployment``.
   -  project\_name = The name of the Ansible project. By default, this
      will use the name of the directory that the ``container.yml`` file
      is in.

-  services = This is where one or more docker containers are defined. A
   unique name should be provided to each different container. These
   names are used as the hosts in the Playbook file.

Syntax:

 .. code-block:: yaml

   services:
     <GROUP_OR_HOST>:

Example:

.. code-block:: yaml

  services:
    mysql:

-  from = The docker image to use for a service.

Syntax:

.. code-block:: yaml

  from: "<IMAGE>:<VERSION>"

Example:

.. code-block:: yaml

  from: "ubuntu:xenial"

-  roles = A list of Ansible roles to run on the container.
-  ports = The hypervisor port to bind to and the container port to
      forward traffic to/from.

Syntax:


.. code-block:: yaml

  ports: <DOCKER_PORT>:<HYPERVISOR_PORT>

Example:


.. code-block:: yaml

  ports: "4444:443"


-  expose = Similar to `ports` but the port forwarding is only done on the hypervisor's localhost address.
-  links = Directly connect container networks for container-to-container traffic.
-  command = Specify a shell command, providing all of the arguments separated via a list. This is the default command run to start the container. If this command stops then the container will be stopped.

Syntax:


.. code-block:: yaml

   command:
     - <COMMAND>
     - <ARGUMENT_1>
     - <ARGUMENT_2>
     - <ARGUMENT_3>

-  entrypoint = Specify a shell command to run before starting the main `command`. This allows for checks to ensure dependencies are running.
-  depends_on = The services/containers that this container requires before starting. This helps to start services in a specific sequence.
-  volumes = Define all of the bind mounts from the hypervisor to the docker container.
-  volumes\_from = Mount some or all all the same mounts that another container is using.

The docker container(s) can be created after the ``container.yml`` file
is completed to describe the container deployment.

.. code-block:: sh

    $ ansible-container build

[25]

Dashboards
----------

Various dashboards are available that provide a graphical user interface
(GUI) and usually an API to help automate Ansible deployments. These can
be used for user access control lists (ACLs), scheduling automated
tasks, and having a visual representation of Ansible's deployment
statistics.

Ansible Tower 3
~~~~~~~~~~~~~~~

Ansible Tower is the official dashboard maintained by Red Hat. The
program requires PostgreSQL, Python, and RabbitMQ. A free trial of it
can be used to manage up to 10 servers for testing purposes only. A
license can be bought to use Tower for managing more servers and to
provide support.

Ansible Tower 3.1 Requirements:

-  Red Hat Enterprise Linux (RHEL) 7, Ubuntu 14.04, and Ubuntu 16.04

   -  Support for RHEL 6 was dropped in 3.1.0

-  Ansible >= 2.1
-  PostgreSQL 9.4

Ansible Tower 3.2 Requirements:

-  Red Hat Enterprise Linux (RHEL) 7, Ubuntu 14.04, and Ubuntu 16.04
-  Ansible >= 2.3
-  Access to some inventory sources, including Azure, requires >= 2.4.
-  PostgreSQL 9.6

Tower can be downloaded from http://releases.ansible.com/ansible-tower/.
The "setup" package only contains Ansible Tower. The "setup-bundle" has
all of the dependencies for offline installation on RHEL servers. At
least a free trial license will be required to use the software which
can be obtained from the `Ansible Tower license
page <https://www.ansible.com/license>`__.

Once downloaded, it can be installed. This will at least setup a Nginx
server and a virtual host for serving Ansible Tower via HTTP.

.. code-block:: sh

    $ curl -O http://releases.ansible.com/ansible-tower/setup/ansible-tower-setup-latest.tar.gz
    $ tar -x -z -v -f ansible-tower-setup-latest.tar.gz
    $ cd ./ansible-tower-setup-3.*/

At a bare minimum for 1 node Ansible Tower installation, the passwords
to use for the various services need to be defined in the ``inventory``
file.

-  admin\_password
-  pg\_password
-  rabbitmq\_password

Ansible Tower supports clustering. This requires that the PostgreSQL
service is configured on a dedicated server that is not running Ansible
Tower. The Playbook that installs Tower can install PostgreSQL or use
credentials to an existing server. The PostgreSQL user for Ansible Tower
also requires ``CREATEDB`` access during the initial installation to
setup the necessary database and tables.

-  Installing PostgreSQL:

.. code-block:: ini

    [tower]
    <TOWER1>
    <TOWER2>
    <TOWER3>

    [database]
    <POSTGRESQL1>

    [all:vars]
    # PostgreSQL
    pg_database="awx"
    pg_username="awx"
    pg_password="<PASSWORD>"
    # RabbitMQ
    rabbitmq_port=5672
    rabbitmq_vhost="tower"
    rabbitmq_username="tower"
    rabbitmq_password="<PASSWORD>"
    rabbitmq_cookie="<RANDOM_STRING>"
    ## Set to "True" if fully qualified domain names (FQDNs)
    ## are used in the inventory file. Otherwise this should
    ## be "False"
    rabbitmq_use_long_name="True"

-  Using an existing PostgreSQL server:

.. code-block:: ini

    [tower]
    <TOWER1>
    <TOWER2>
    <TOWER3>

    [all:vars]
    # PostgreSQL
    pg_host="<HOST_OR_IP>"
    pg_port=5432
    pg_database="awx"
    pg_username="awx"
    pg_password="<PASSWORD>"
    # RabbitMQ
    rabbitmq_port=5672
    rabbitmq_vhost="tower"
    rabbitmq_username="tower"
    rabbitmq_password="<PASSWORD>"
    rabbitmq_cookie="<RANDOM_STRING>"
    ## Set to "True" if fully qualified domain names (FQDNs)
    ## are used in the inventory file. Otherwise this should
    ## be "False"
    rabbitmq_use_long_name="True"

The "ansible" package needs to be available in a package repository. On
RHEL systems, the Extra Packages for Enterprise Linux (EPEL) repository
should be installed.

.. code-block:: sh

    $ sudo yum -y install epel-release

Then install Ansible Tower using the setup shell script. This will run
an Ansible Playbook to install Tower.

.. code-block:: sh

    $ sudo ./setup.sh

When the installation is complete, Ansible Tower can be accessed by a
web browser. If no SSL certificate was defined, then a self-signed SSL
certificate will be used to encrypt the connection. The default username
is "admin" and the password is defined in the ``inventory`` file.

::

    https://<SERVER_IP_OR_HOSTNAME>/

For updating Ansible Tower, download the latest tarball release and
re-run the ``setup.sh`` script with the original inventory and
variables. Adding new nodes to the cluster should also have that script
run again so that all of the existing and new nodes will be configured
to know about each other. Automatically scaling and/or replicating
PostgreSQL is currently not supported by the Tower setup Playbook. Only
1 database node can be configured by this Playbook.

Logs are stored in ``/var/log/tower/``. The main log file is
``/var/log/tower/tower.log``.

Ports:

-  80/tcp = Unencyrpted Ansible Tower dashboard web traffic.
-  443/tcp = SSL encrypted Ansible Tower dashboard web traffic.
-  5432/tcp = PostgreSQL relational database server.

Cluster ports:

-  4369/tcp = Discovering other Ansible Tower nodes.
-  5672/tcp = Local RabbitMQ port.
-  25672/tcp = External RabbitMQ port.
-  15672/tcp = RabbitMQ dashboard.

[35][37]

GUI
^^^

There is a navigation bar that contains links to the most important
parts of Ansible Tower.

-  Projects = Playbooks and, if applicable, credentials to access them
   from different types of source code management (SCM) systems.

   -  Manual = Use the a Playbook from the local file system on the
      Ansible Tower server.
   -  Git = A SCM.
   -  Mercurial (hg) = A SCM.
   -  Subversion (svn) = A SCM.
   -  Red Hat Insights = Use a Playbook from the Red Hat Insights
      program to do validation checks.

-  Inventories = Static and dynamic inventories can be defined here.

   -  Supported sources:

      -  Custom Script = Provide a custom script that outputs a valid
         JSON or YAML inventory.
      -  Sourced from a Project = Import inventory from an existing
         Project (Playbook).
      -  Amazon EC2
      -  Google Compute Engine
      -  Microsoft Azure Resource Manager
      -  VMWare vCenter
      -  Red Hat Satellite 6
      -  OpenStack

-  Templates = Used for defining a Playbook to run, the hosts to run on,
   any additional variables to use, and optionally a time interval to
   automatically run the template.

   -  Workflow Template = Defines a workflow for determining when to run
      a Playbook and what runs after depending on if the Playbook failed
      or succeeded.

-  Jobs = Templates that have been run (or are running) and their logs
   and statistics.
-  (The gear/cog image) = Settings for configuring new users, teams,
   dynamic inventory scripts, notifications, the license, and other
   settings relating to the Ansible Tower installation.
-  (The book image) = A shortcut to Ansible Tower's official
   documentation.

[50][51]

The default timeout for an authorization token is 30 minutes. After this
time, the token will expire and the end-user will need to re-login into
their account. This setting can be modified in the ``settings.py`` file.
[52]

.. code-block:: sh

    $ sudo vim /etc/tower/conf.d/settings.py
    AUTH_TOKEN_EXPIRATION = <SECONDS_BEFORE_TIMEOUT>

Security
^^^^^^^^

Authentication
''''''''''''''

User authentication, by default, will store encrypted user information
into the PostgreSQL database. Instead of using this, Tower can be
configured to use an external authentication service by going into
``Settings > CONFIGURE TOWER``. The available options are:

-  Social

   -  GitHub

      -  GitHub [Users]
      -  GitHub Org[anization]
      -  GitHub Team

   -  Google OAuth2

-  Enterprise

   -  Azure AD
   -  LDAP
   -  RADIUS
   -  SAML
   -  TACACS+

[52]

ACLs
''''

Every user in Tower is associated with at least one organization. The
level of access the user has to that organizations resources is defined
by one of the different access control lists (ACLs).

Hierarchy [1]:

-  Organizations = A combination of Users, Teams, Projects, and
   Inventories.

   -  Teams = Teams are a collection of users. Teams are not required.
      Multiple users' access can be modified easily and quickly via a
      Team instead of individually modifying each user's access.

      -  Users = Users are optionally associated with a Team and are
         always associated with an Organization. An ACL is set for what
         resources the user is allowed to use.

User types / ACLs:

-  System Administrator = Has full access to all organizations and the
   Tower installation.
-  System Auditor = Has read-only access to an organization.
-  Normal User = Has read and write access to an organization.

[51]

SSL
'''

By default, Tower creates a self-signed SSL certificate to secure web
traffic. [37] Most web browsers will mark this as an untrusted
connection. For using a different SSL that is trusted, the contents of
these two files need to be replaced on each Tower node:

-  ``/etc/tower/tower.cert``
-  ``/etc/tower/tower.key``

API
^^^

Ansible Tower has a strong focus on automating Ansible even more by
providing an API interface. Programs can interact with this by making
HTTP GET and PUT requests. All of the available endpoints can be viewed
by going to:

``https://<ANSIBLE_TOWER_HOST>/api/v1/``

Version 1 of the API provides these endpoints:

.. code-block:: json

    {
        "authtoken": "/api/v1/authtoken/",
        "ping": "/api/v1/ping/",
        "config": "/api/v1/config/",
        "settings": "/api/v1/settings/",
        "me": "/api/v1/me/",
        "dashboard": "/api/v1/dashboard/",
        "organizations": "/api/v1/organizations/",
        "users": "/api/v1/users/",
        "projects": "/api/v1/projects/",
        "project_updates": "/api/v1/project_updates/",
        "teams": "/api/v1/teams/",
        "credentials": "/api/v1/credentials/",
        "inventory": "/api/v1/inventories/",
        "inventory_scripts": "/api/v1/inventory_scripts/",
        "inventory_sources": "/api/v1/inventory_sources/",
        "inventory_updates": "/api/v1/inventory_updates/",
        "groups": "/api/v1/groups/",
        "hosts": "/api/v1/hosts/",
        "job_templates": "/api/v1/job_templates/",
        "jobs": "/api/v1/jobs/",
        "job_events": "/api/v1/job_events/",
        "ad_hoc_commands": "/api/v1/ad_hoc_commands/",
        "system_job_templates": "/api/v1/system_job_templates/",
        "system_jobs": "/api/v1/system_jobs/",
        "schedules": "/api/v1/schedules/",
        "roles": "/api/v1/roles/",
        "notification_templates": "/api/v1/notification_templates/",
        "notifications": "/api/v1/notifications/",
        "labels": "/api/v1/labels/",
        "unified_job_templates": "/api/v1/unified_job_templates/",
        "unified_jobs": "/api/v1/unified_jobs/",
        "activity_stream": "/api/v1/activity_stream/",
        "workflow_job_templates": "/api/v1/workflow_job_templates/",
        "workflow_jobs": "/api/v1/workflow_jobs/",
        "workflow_job_template_nodes": "/api/v1/workflow_job_template_nodes/",
        "workflow_job_nodes": "/api/v1/workflow_job_nodes/"
    }

``https://<ANSIBLE_TOWER_HOST>/api/v2/``

Version 2 of the API provides these endpoints:

.. code-block:: json

    {
        "authtoken": "/api/v2/authtoken/",
        "ping": "/api/v2/ping/",
        "instances": "/api/v2/instances/",
        "instance_groups": "/api/v2/instance_groups/",
        "config": "/api/v2/config/",
        "settings": "/api/v2/settings/",
        "me": "/api/v2/me/",
        "dashboard": "/api/v2/dashboard/",
        "organizations": "/api/v2/organizations/",
        "users": "/api/v2/users/",
        "projects": "/api/v2/projects/",
        "project_updates": "/api/v2/project_updates/",
        "teams": "/api/v2/teams/",
        "credentials": "/api/v2/credentials/",
        "credential_types": "/api/v2/credential_types/",
        "inventory": "/api/v2/inventories/",
        "inventory_scripts": "/api/v2/inventory_scripts/",
        "inventory_sources": "/api/v2/inventory_sources/",
        "inventory_updates": "/api/v2/inventory_updates/",
        "groups": "/api/v2/groups/",
        "hosts": "/api/v2/hosts/",
        "job_templates": "/api/v2/job_templates/",
        "jobs": "/api/v2/jobs/",
        "job_events": "/api/v2/job_events/",
        "ad_hoc_commands": "/api/v2/ad_hoc_commands/",
        "system_job_templates": "/api/v2/system_job_templates/",
        "system_jobs": "/api/v2/system_jobs/",
        "schedules": "/api/v2/schedules/",
        "roles": "/api/v2/roles/",
        "notification_templates": "/api/v2/notification_templates/",
        "notifications": "/api/v2/notifications/",
        "labels": "/api/v2/labels/",
        "unified_job_templates": "/api/v2/unified_job_templates/",
        "unified_jobs": "/api/v2/unified_jobs/",
        "activity_stream": "/api/v2/activity_stream/",
        "workflow_job_templates": "/api/v2/workflow_job_templates/",
        "workflow_jobs": "/api/v2/workflow_jobs/",
        "workflow_job_template_nodes": "/api/v2/workflow_job_template_nodes/",
        "workflow_job_nodes": "/api/v2/workflow_job_nodes/"
    }

[36]

AWX
~~~

AWX is the upstream and open source version of Ansible Tower released by
Red Hat to the public on September 7, 2017. [41] The source code for the
project can be found in the
`ansible/awx <https://github.com/ansible/awx>`__ repository on GitHub.

Install
^^^^^^^

The "installer/inventory" file has an example inventory that can be used
without any configuration. These are many options that can be fine tuned
to change the environment and deploy it to different platforms.

Deployment inventory options:

-  All

   -  default\_admin\_user = The administrator account's username.
   -  default\_admin\_password = The administrator account's password.
   -  awx\_secret\_key = A string that is used as a private key kept on
      all of the AWX nodes for encrypting and decrypting information
      that goes to/from the PostgreSQL database.
   -  pg\_username = The PostgreSQL username to use.
   -  pg\_password = The PostgreSQL password to use.
   -  pg\_database = The PostgreSQL database name.
   -  pg\_port = The PostgreSQL port to connect to.
   -  http\_proxy = A HTTP proxy to use.
   -  https\_proxy = A HTTPS proxy to use.
   -  no\_proxy = Websites that should not be proxied.

-  docker (build)

   -  dockerhub\_base and dockerhub\_version = Comment out these
      variables to build docker images from scratch instead of
      downloading them from Docker Hub.
   -  postgres\_data\_dir = The directory to store persistent PostgreSQL
      data. By default, this is stored in the temporary file system at
      ``/tmp/``.
   -  host\_port = The local HTTP port that docker should bind to for
      the AWX dashboard.
   -  use\_container\_for\_build = Use a container to deploy the other
      container. This keeps dependencies installed for installation in
      the container instead of the local system.
   -  awx\_official = Use the official AWX logos. The `awx-logos GitHub
      project <https://github.com/ansible/awx-logos>`__ will need to be
      cloned in the directory that "awx" was also cloned into.

-  docker (prebuilt)

   -  dockerhub\_base = The Docker Hub repository to use.
   -  dockerhub\_version = The version of AWX containers to use.
   -  postgres\_data\_dir
   -  host\_port

-  OpenShift

   -  openshift\_host = The OpenShift cluster to connect to.
   -  openshift\_user = The username used to connect to OpenShift.
   -  openshift\_password = The password used to connect to OpenShift.
   -  docker\_registry = The Docker Registry to connect to.
   -  docker\_registry\_repository = The Docker Repository to use.
   -  docker\_registry\_username = The username to login into the Docker
      Registry.
   -  docker\_registry\_password = The password to login into the Docker
      Registry.
   -  awx\_openshift\_project = The name of the project to create and
      use in OpenShift.
   -  awx\_node\_port = The HTTP port to use inside of the AWX pod for
      accessing the dashboard.

Install:

By default, AWX will install docker containers from Docker Hub. It is
also possible to have the installer build docker containers from scratch
an deploy them into a OpenShift cluster.

.. code-block:: sh

    $ git clone https://github.com/ansible/awx.git
    $ cd ./awx/installer/
    $ sudo ansible-playbook -i inventory install.yml

After installation, the containers will be started. On a server, the
containers are configured to automatically restart up on boot if the
``docker`` service is enabled. With workstations and other environments
where docker is not running on boot, the containers can still be started
and stopped manually.

Manually start:

.. code-block:: sh

    $ for docker_container in postgres rabbitmq memcached awx_web awx_task; do sudo docker start ${docker_container}; done

Manually stop:

.. code-block:: sh

    $ for docker_container in awx_task awx_web memcached rabbitmq postgres; do sudo docker stop ${docker_container}; done

Upgrades
^^^^^^^^

AWX is the unstable development version of Ansible Tower. **Major version upgrades are not supported** (for example, going from 1.0.4 to 1.0.5). [60] Work is being done in the `tower-cli <https://github.com/ansible/tower-cli>`__ project to allow backing up and restoring configurations in AWX. Once that work is done, it will be the most reliable way to attempt an upgrade. [61] It is still possible to attempt updating the service containers.

Take note of the AWX version. This can be seen through the dashboard or API. If AWX was installed using the GitHub source code and Docker Compose, the version can be found from the Git description.

.. code-block:: sh

    $ git describe --long --first-parent | cut -d- -f1,2

Before doing an upgrade, the PostgreSQL database should be backed up. This contains all of the information that is stored by AWX.

.. code-block:: sh

    $ docker exec postgres pg_dump -U postgres -F t awx > ${postgres_data_dir}/awx_backup.sql

If there are issues with the update, then revert the docker images to their original versions and restore the database.

.. code-block:: sh

    $ docker exec postgres sh -c "pg_restore -U awx -C -d awx /var/lib/postgresql/data/awx_backup.sql"

Compare the latest "installer/inventory" file with the existing inventory file. Some settings may have changed that need to be updated.

Update AWX by re-running the installer Playbook from the latest clone of the GitHub repository clone. The default installation settings for using Docker Hub will download the latest images.

.. code-block:: sh

    $ git clone https://github.com/ansible/awx
    $ ansible-playbook -i inventory awx/installer/install.yml

[59]

Uninstall
^^^^^^^^^

Stop and delete the AWX docker containers.

.. code-block:: sh

    $ for docker_container in awx_task awx_web memcached rabbitmq postgres; do docker stop ${docker_container}; docker rm ${docker_container}; done

The "postgres_data_dir" directory, as defined in the inventory file, will need to be manually deleted.

Rundeck
~~~~~~~

Rundeck is an open source dashboard and API framework, programmed with
Java, for automating the execution of commands and scripts via SSH.
There is a community supported Ansible plugin for Rundeck that is
currently only *alpha* quality. It supports using Ansible inventory as
well as running modules and Playbooks. This can be tested out using a
pre-built docker image:

.. code-block:: sh

    $ sudo docker pull batix/rundeck-ansible
    $ sudo docker run -d --name rundeck-test -p 127.0.0.1:4440:4440 -e RDECK_ADMIN_PASS=password -v `pwd`:/data batix/rundeck-ansible

Log into the dashboard at ``http://127.0.0.1:4440`` and use the username
"admin" and the password that was set by the ``RDECK_ADMIN_PASS``
variable.

[56]

Semaphore
~~~~~~~~~

Semaphore was designed to be an unofficial open source alternative to
Ansible Tower, written in Go. The latest release can be found at
https://github.com/ansible-semaphore/semaphore/releases.

Requirements:

-  Ansible
-  Git >= 2.0
-  Go
-  MariaDB >= 5.3 or MySQL >= 5.6.4

Installation:

.. code-block:: sh

    $ sudo curl -L https://github.com/ansible-semaphore/semaphore/releases/download/v2.4.1/semaphore_linux_amd64 > /usr/bin/semaphore
    $ sudo /usr/bin/semaphore -setup

Semaphore will now be available at ``http://<SEMAPHORE_HOST>:3000``.

[26]

Tensor
~~~~~~

Tensor is a open source dashboard and API for both Ansible and
Terraform, written in Go by Pearson. Most of the public facing
documentation on it's GitHub page is either incomplete or missing.

Requirements:

-  Ansible 2
-  Git
-  Go
-  MongoDB 3.3

The ``Makefile`` supports building packages for Debian and RHEL based
distributions as well creating docker containers.

.. code-block:: sh

    $ make deb

.. code-block:: sh

    $ make rpm

.. code-block:: sh

    $ make docker

[57]

`Errata <https://github.com/ekultails/rootpages/commits/master/src/ansible.rst>`__
----------------------------------------------------------------------------------

Bibliography
------------

1. "An Ansible Tutorial." Servers for Hackers. August 26, 2014. Accessed June 24, 2016. https://serversforhackers.com/an-ansible-tutorial
2. "Intro to Playbooks." Ansible Documentation. August 4, 2017. Accessed August 6, 2017. http://docs.ansible.com/ansible/playbooks\_intro.html
3. "Inventory." Ansible Docs. June 22, 2016. Accessed July 9, 2016. http://docs.ansible.com/ansible/intro\_inventory.html
4. "Variables." Ansible Documentation. June 1, 2017. Accessed June 17, 2017. http://docs.ansible.com/ansible/playbooks\_variables.html
5. "Best Practices." Ansible Documentation. June 4, 2017. Accessed June 4, 2017. http://docs.ansible.com/ansible/playbooks\_best\_practices.html
6. "Commands Modules." Ansible Documentation. October 10, 2017. Accessed March 2, 2018. http://docs.ansible.com/ansible/latest/list\_of\_commands_modules.html
7. "Source Control Modules." Ansible Documentation. October 10, 2017. Accessed March 2, 2018. http://docs.ansible.com/ansible/latest/list\_of\_source\_control\_modules.html
8. "Tags." Ansible Documentation. April 21, 2017. Accessed April 22, 2017. http://docs.ansible.com/ansible/playbooks\_tags.html
9. "Prompts." Ansible Documentation. August 05, 2016. Accessed August 13, 2016. http://docs.ansible.com/ansible/playbooks\_prompts.html
10. "Loops." Ansible Documentation. April 12, 2017. Accessed April 13, 2017. http://docs.ansible.com/ansible/playbooks\_loops.html
11. "Conditionals." Ansible Documentation. April 12, 2017. Accessed April 13, 2017. http://docs.ansible.com/ansible/playbooks\_conditionals.html
12. "Error Handling In Playbooks." Ansible Documentation. August 24, 2016. Accessed August 27, 2016. http://docs.ansible.com/ansible/playbooks\_error\_handling.html
13. "Become (Privilege Escalation)." Ansible Documentation. August 24, 2016. Accessed August 27, 2016. http://docs.ansible.com/ansible/become.html
14. "Delegation, Rolling Updates, and Local Actions." Ansible Documentation. April 12, 2017. Accessed April 13, 2017. http://docs.ansible.com/ansible/playbooks\_delegation.html
15. "Asynchronous Actions and Polling." Ansible Documentation. September 1, 2016. Accessed September 11, 2016. http://docs.ansible.com/ansible/playbooks\_async.html
16. "mysql\_db - Add or remove MySQL databases from a remote host." Ansible Documentation. September 28, 2016. Accessed October 1, 2016. http://docs.ansible.com/ansible/mysql\_db\_module.html
17. "mysql\_user - Adds or removes a user from a MySQL database." Ansible Documentation. September 28, 2016. Accessed October 1, 2016. http://docs.ansible.com/ansible/mysql\_user\_module.html
18. "Ansible Installation." Ansible Documentation. October 10, 2016. Accessed October 16, 2016. http://docs.ansible.com/ansible/intro\_installation.html
19. "Ansible 2.2.0 RC1 is ready for testing." Ansible Development Group. October 3, 2016. Accessed October 16, 2016. https://groups.google.com/forum/#!searchin/ansible-devel/python$203$20support%7Csort:relevance/ansible-devel/Ca07JSmyxIQ/YjFfbb8TAAAJ
20. "Jinja Template Designer Documentation." Jinja2 Documentation. Accessed April 23, 2017. http://jinja.pocoo.org/docs/dev/templates/
21. "Ansible Vault." Ansible Documentation. October 10, 2017. Accessed March 2, 2018. http://docs.ansible.com/ansible/latest/vault.html
22. "Organizing Group Vars Files in Ansible." toja.io sysadmin, devops and videotapes. Accessed November 6, 2016. http://toja.io/using-host-and-group-vars-files-in-ansible/
23. "Glossary." Ansible Documentation. October 31, 2016. Accessed November 12, 2016. http://docs.ansible.com/ansible/intro\_installation.html
24. "Ansible Container README." Ansible GitHub. October, 2016. Accessed November 19, 2016. https://github.com/ansible/ansible-container
25. "Ansible Container." Ansible Documentation. June 3, 2017. Accessed June 3, 2017. http://docs.ansible.com/ansible-container/
26. "Semaphore Installation." GitHub - ansible-semaphore/semaphore. June 1, 2017. Accessed August 14, 2017. https://github.com/ansible-semaphore/semaphore/wiki/Installation
27. "Ansible Galaxy." Ansible Documentation. March 31, 2017. Accessed April 4, 2017. http://docs.ansible.com/ansible/galaxy.html
28. "ANSIBLE PERFORMANCE TUNING (FOR FUN AND PROFIT)." Ansible Blog. July 10, 2014. Accessed January 25, 2017. https://www.ansible.com/blog/ansible-performance-tuning
29. "Configuration file." Ansible Documentation. April 17, 2017. Accessed April 20, 2017. http://docs.ansible.com/ansible/intro\_configuration.html
30. "network\_interface." MartinVerges GitHub. January 24, 2017. Accessed April 4, 2017. https://github.com/MartinVerges/ansible.network\_interface
31. "Check Mode ("Dry Run")." Ansible Documentation. April 12, 2017. Accessed April 13, 2017. http://docs.ansible.com/ansible/playbooks\_checkmode.html
32. "Return Values." Ansible Documentation. April 17, 2017. Accessed April 18, 2017. http://docs.ansible.com/ansible/common\_return\_values.html
33. "Windows Support." Ansible Documentation. August 4, 2017. Accessed August 10, 2017. http://docs.ansible.com/ansible/latest/intro\_windows.html
34. "Ansible Python API." Ansible Documentation. March 29, 2018. Accessed March 30, 2018. http://docs.ansible.com/ansible/latest/dev\_guide/developing\_api.html
35. "Installing and Configuring Ansible Tower Clusters - AnsbileFest London 2017." YouTube - Ansible. July 19, 2017. Accessed August 10, 2017. https://www.youtube.com/watch?v=NiM4xNkauig
36. "Ansible Tower API Guide." Ansible Documentation. Accessed October 2, 2017. http://docs.ansible.com/ansible-tower/latest/html/towerapi/index.html
37. "Ansible Tower Installation and Reference Guide." Ansible Documentation. February 20, 2018. Accessed March 2, 2018. http://docs.ansible.com/ansible-tower/latest/html/installandreference/index.html
38. "Ansible Strategies." Ansible Documentation. August 16, 2017. Accessed August 24, 2017. http://docs.ansible.com/ansible/latest/playbooks\_strategies.html
39. "Get-WindowsFeature." MSDN Library. November 1, 2013. Accessed August 6, 2017. https://msdn.microsoft.com/en-us/library/ee662312.aspx
40. "Ansible Tower Job Templates." Ansible Tower Documentation. Accessed September 7, 2017. http://docs.ansible.com/ansible-tower/latest/html/userguide/job\_templates.html
41. "Ansible announces AWX open source project." OpenSource.com. September 7, 2017. Accessed September 7, 2017. https://opensource.com/article/17/9/ansible-announces-awx-open-source-project
42. "Red Hat Ansible Engine." Ansible. Accessed September 12, 2017. https://www.ansible.com/ansible-engine
43. "Ansible Python 3 Support." Ansible Documentation. September 12, 2017. Accessed September 14, 2017. http://docs.ansible.com/ansible/latest/python\_3\_support.html
44. "Ansible [README.md]." Ansible GitHub. September 14, 2017. Accessed September 18, 2017. https://github.com/ansible/ansible
45. "Utilities Modules." Ansible Documentation. September 18, 2017. Accessed September 26, 2017. http://docs.ansible.com/ansible/latest/list\_of\_utilities\_modules.html
46. "Files Modules." Ansible Documentation. September 18, 2017. Accessed September 21, 2017. http://docs.ansible.com/ansible/latest/list\_of\_files\_modules.html
47. "Packaging Modules." Ansible Documentation. September 18, 2017. Accessed September 21, 2017. http://docs.ansible.com/ansible/latest/list\_of\_packaging\_modules.html
48. "Windows Modules." Ansible Documentation. September 18, 2017. Accessed September 21, 2017. http://docs.ansible.com/ansible/latest/list\_of\_windows\_modules.html
49. "Creating Reusable Playbooks." Ansible Documentation. September 18, 2017. Accessed September 21, 2017. http://docs.ansible.com/ansible/latest/playbooks\_reuse.html
50. "Ansible Tower Quick Setup Guide." Ansible Documentation. September 18, 2017. Accessed September 25, 2017. http://docs.ansible.com/ansible-tower/latest/html/quickstart/index.html
51. "Ansible Tower User Guide." Ansible Documentation. September 18, 2017. Accessed September 25, 2017. http://docs.ansible.com/ansible-tower/latest/html/userguide/index.html
52. "Ansible Tower Administration Guide." Ansible Documentation. September 18, 2017. Accessed September 25, 2017. http://docs.ansible.com/ansible-tower/latest/html/administration/index.html
53. "Blocks." Ansible Documentation. September 18, 2017. Accessed September 26, 2017. http://docs.ansible.com/ansible/latest/playbooks\_blocks.html
54. "Net Tools Modules." Ansible Documentation. September 18, 2017. Accessed September 26, 2017. http://docs.ansible.com/ansible/latest/list\_of\_net\_tools\_modules.html
55. "System Modules." Ansible Documentation. September 18, 2017. Accessed September 26, 2017. http://docs.ansible.com/ansible/latest/list\_of\_system\_modules.html
56. "Rundeck Ansible Plugin [README.md]." Batix GitHub. August 9, 2017. Accessed September 26, 2017. https://github.com/Batix/rundeck-ansible-plugin
57. "Tensor [README.md]." PearsonAppEng GitHub. April 25, 2017. Accessed September 26, 2017. https://github.com/pearsonappeng/tensor
58. "Including and Importing." Ansible Documentation. October 10, 2017. Accessed March 2, 2018. http://docs.ansible.com/ansible/latest/playbooks\_reuse\_includes.html
59. "Specify a PGDATA directory to prevent container re-create issues #535." GitHub Ansible. February 22, 2018. Accessed March 9, 2018. https://github.com/ansible/awx/pull/535
60. "1.0.4 - 1.0.5 upgrade killed my workers & errors db upgrade Key (username)=(admin) already exists. #1707." GitHub AWX. March 30, 2018. Accessed April 6, 2018. https://github.com/ansible/awx/issues/1707
61. "Feature to download & upload data in Tower #197." GitHub tower-cli. February 28, 2018. Accessed April 6, 2018. https://github.com/ansible/tower-cli/issues/197
