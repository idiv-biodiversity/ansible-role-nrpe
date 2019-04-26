Ansible Role: Nagios Remote Plugin Executor (NRPE)
==================================================

An Ansible role that installs and configures [NRPE][].

Table of Contents
-----------------

<!-- toc -->

- [Requirements](#requirements)
- [Role Variables](#role-variables)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
  * [Top-Level Playbook](#top-level-playbook)
  * [Role Dependency](#role-dependency)
- [License](#license)
- [Author Information](#author-information)

<!-- tocstop -->

Requirements
------------

- Ansible 2+

Role Variables
--------------

These are all variables and their default values:

```yml
nrpe_log_facility: 'daemon'

nrpe_pid_file: '/var/run/nrpe/nrpe.pid'

nrpe_server_port: '5666'

nrpe_user: 'nrpe'

nrpe_group: 'nrpe'

nrpe_allowed_hosts:
  - '127.0.0.1'
  - '::1'

nrpe_dont_blame: '0'

nrpe_allow_bash_command_substitution: '0'

nrpe_debug: '0'

nrpe_command_timeout: '60'

nrpe_connection_timeout: '300'

nrpe_commands:
  - name: 'check_users'
    line: '/usr/lib64/nagios/plugins/check_users -w 5 -c 10'

  - name: 'check_load'
    line: '/usr/lib64/nagios/plugins/check_load -r -w .15,.10,.05 -c .30,.25,.20'

  - name: 'check_hda1'
    line: '/usr/lib64/nagios/plugins/check_disk -w 20% -c 10% -p /dev/hda1'

  - name: 'check_zombie_procs'
    line: '/usr/lib64/nagios/plugins/check_procs -w 5 -c 10 -s Z'

  - name: 'check_total_procs'
    line: '/usr/lib64/nagios/plugins/check_procs -w 150 -c 200'

```

Dependencies
------------

This role *conditionally* depends on [geerlingguy.repo-epel][repo-epel] for **RedHat**-based distributions to install runtime and build dependencies. Not all of these dependencies are included in default repositories.

Example Playbook
----------------

Add to `requirements.yml`:

```yml
---

# optional
# - src: geerlingguy.repo-epel

- src: idiv-biodiversity.nrpe

...
```

Download:

```console
$ ansible-galaxy install -r requirements.yml
```

### Top-Level Playbook

Write a top-level playbook:

```yml
---

- name: head server
  hosts: head

  roles:
    - role: idiv-biodiversity.nrpe
      tags:
        - icinga
        - nagios
        - nrpe

...
```

### Role Dependency

Define the role dependency in `meta/main.yml`:

```yml
---

dependencies:

  - role: idiv-biodiversity.nrpe
    tags:
      - icinga
      - nagios
      - nrpe

...
```

License
-------

MIT

Author Information
------------------

This role was created in 2017 by [Christian Krause][author] aka [wookietreiber
at GitHub][wookietreiber], HPC cluster systems administrator at the [German
Centre for Integrative Biodiversity Research (iDiv)][idiv], based on a draft by
Ben Langenberg aka [sloan87 at GitHub][sloan87].


[author]: https://www.idiv.de/groups_and_people/employees/details/eshow/krause-christian.html
[idiv]: https://www.idiv.de/
[sloan87]: https://github.com/sloan87
[wookietreiber]: https://github.com/wookietreiber
[NRPE]: https://github.com/NagiosEnterprises/nrpe
[repo-epel]: https://galaxy.ansible.com/geerlingguy/repo-epel/
