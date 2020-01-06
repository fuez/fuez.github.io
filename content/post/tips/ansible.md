---
title: "ansible"
date: 2017-06-13T11:48:45+08:00
lastmod: 2019-11-13T16:38:07+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## [How to run ansible adhoc command?](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html)
> `ansible <targets> -m ping` to ping targets
> `ansible <targets> -a 'systemctl status'` to run a command on targets
> `ansible <targets> -m user -a "name=foo state=absent` to run a ansible module on targets

## How to reboot and wait for linux server to come back?

Sample code is as the following:

```yaml
## name: Upgrade Kernel | Reboot server
  command: 'sleep 5 && /sbin/reboot'
  async: 10
  poll: 0
 
## name: Waiting for server to come back
  local_action: wait_for host={{ ansible_ssh_host }} state=started
```

*Note*: it is important to sleep for a while before rebooting, otherwise, async seems not working, maybe because ssh connection is suddenly lost before async takes affect.
And for ansible >= 2.7, there is module called [reboot](https://docs.ansible.com/ansible/latest/modules/reboot_module.html) for the same purpose.

## [A lot of IP address operation such as `ipaddr`](https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters_ipaddr.html)

## It is possible to add more vars such as `ansible_host` to inventory host declaration as stated in [ansible doc](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)

## It is important to put `groups_vars` under `inventory` folder, otherwise, `ansible-playbook` will try to find vars under the same folder as playbook yaml file.

## How to pass list or dict like configurations to an INI style inventory file?

Keep in mind that values in vars will be parsed/evaluated by `python`, so the following additional var should be valid (and it is possible to convert from and conver to json/yaml by jinja2 filters):

```ini
[osds:vars]
lvm_volumes=[{'data': '/dev/sdb'}, {'data': '/dev/sdc'}]
```

## Understand `replace` module

```yaml
  replace:
    path: /etc/default/grub
    regexp: '^(GRUB_DEFAULT)=.*$'
    replace: '\1=0'
    backup: yes
```

In the above action, the regexp will capture `GRUB_DEFAULT` as the first group, which will be used in `'\1=0`, change this line to `GRUB_DEFAULT=0`

## Ansible provide [debugger](https://docs.ansible.com/ansible/latest/user_guide/playbooks_debugger.html) for better trouble shooting.
When trapped in debugger, you can use `p` command to print current task state, available state includes: `result._result, task, task.args, task_vars`, and you can also change those states with format like `task.args[key] = value`. And then you can `redo(r)` or `continue(c)`, or just `quit(q)`. When `p` vars, it is possible to print like this `p task_vars.keys()` since `task_vars` here is a python dict object, `p task_vars['hostvars']['osd0']['lvm_volumes']`

## Understand `run_once` parameter by [10 Things you should start using in your Ansible Playbook](https://medium.com/@abhijeet.kamble619/10-things-you-should-start-using-in-your-ansible-playbook-808daff76b65)

>By default, Ansible will select the first host from the group to execute. This can also be used with delegate_to to run the task on specific server. 


## [Latest ansible configure file](https://raw.githubusercontent.com/ansible/ansible/devel/examples/ansible.cfg)

## It is possibe to set environment variables in playbook by adding `environment` section like this:

```yaml
## hosts: all
  remote_user: root
  # here we make a variable named "proxy_env" that is a dictionary
  vars:
    proxy_env:
      http_proxy: http://proxy.example.com:8080
  tasks:

    - apt: name=cobbler state=installed
      environment: "{{proxy_env}}"
```

## About [jinja2 template filters](http://jinja.pocoo.org/docs/2.10/templates/), and for [ansible specific](https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html)

## How to profile ansible playbook?

As of Ansible 2.0, just add `callback_whitelist = profile_tasks` to your ansible.cfg file [defaults] section, which is one type of `callback_plugins` for ansible.

## Pay attention to "meta" directory in ansible role, because role's dependencies will be defined there

## How to fix error: *python2 bindings for rpm are needed for this module*?

Possible root cause is that default python is configured as python3 on the remote machine. Just add `ansible_python_interpreter=/usr/bin/python2.7` var.

## Understand [`block`](https://docs.ansible.com/ansible/latest/user_guide/playbooks_blocks.html) in ansible playbook

> Blocks allow for logical grouping of tasks and in play error handling.Most of what you can apply to a single task (with the exception of loops) can be applied at the block level, which also makes it much easier to set data or directives common to the tasks. This does not mean the directive affects the block itself, but is inherited by the tasks enclosed by a block. i.e. a when will be applied to the tasks, not the block itself.

## What does [`ansible_ssh_pipelining`](http://toroid.org/ansible-ssh-pipelining) means?

>Ansible will normally create a temporary directory under ~/.ansible (via ssh), then for each task, copy the module source to the directory (using sftp or scp) and execute the module (ssh again).
With pipelining enabled, Ansible will connect only once per task using ssh to execute python, and write the module source to its stdin.

## [Variable precedence: Where should I put a variable?](https://docs.ansible.com/ansible/2.7/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable)

Here is the order of precedence from least to greatest (the last listed variables winning prioritization):

```ini
## command line values (eg “-u user”)
## role defaults [1]
## inventory file or script group vars [2]
## inventory group_vars/all [3]
## playbook group_vars/all [3]
## inventory group_vars/* [3]
## playbook group_vars/* [3]
## inventory file or script host vars [2]
## inventory host_vars/* [3]
## playbook host_vars/* [3]
## host facts / cached set_facts [4]
## play vars
## play vars_prompt
## play vars_files
## role vars (defined in role/vars/main.yml)
## block vars (only for tasks in block)
## task vars (only for the task)
## include_vars
## set_facts / registered vars
## role (and include_role) params
## include params
## extra vars (always win precedence)
```

## How to suppress retry file?
> add the following configuration to ansible.cfg *defaults* section:
retry_files_enabled = False

## [Ansible meta module](https://docs.ansible.com/ansible/2.7/modules/meta_module.html)

>Meta tasks are a special kind of task which can influence Ansible internal execution or state.
flush_handlers makes Ansible run any handler tasks which have thus far been notified.

## Ansible configure file seach paths:

>Changes can be made and used in a configuration file which will be processed in the following order:
ANSIBLE_CONFIG (an environment variable)
ansible.cfg (in the current directory)
.ansible.cfg (in the home directory)
/etc/ansible/ansible.cfg

## [Install ansible-roles-graph to show roles dependencies](https://github.com/sebn/ansible-roles-graph)

```sh
# Install graphvis from http://graphviz.org/Download_macos.php
pip install ansible-roles-graph
```

NOT WORKING.
Try another one called [ansigenome](https://github.com/nickjj/ansigenome)

## [About hosts pattern match](http://docs.ansible.com/ansible/latest/intro_patterns.html#patterns)

host match is separted by colon.

could be written as: *hosts: kube-master[0]*  to only play on the first kube-master node
could be written as *hosts: etcd:k8s-cluster:vault:calico-rr* to play on multiple types of nodes

## How to access environment varibles on remote machine?

Could be accessed by: `{{ ansible_var.<Var name> }}` such as `{{ ansible_var.USER }}`

## For jinja2 template, how to evaluate nested variables?

Wrong template file:

```py
{% for i in range(groups["peer"]|length)  %}
       - "peer{{i}}.rong.easy-sight.com:{{ groups['peer'][{{i}}] }}"
{%endfor%}
```

**Should not use double brace in nested style**.
Correct one:

```py
{% for i in range(groups["peer"]|length)  %}
       - "peer{{i}}.rong.easy-sight.com:{{ groups['peer'][i] }}"
{%endfor%}
```

## Trouble shooting the following error:

>fatal: [dev15]: FAILED! => {"failed": true, "msg": "Timeout (12s) waiting for privilege escalation prompt: "}

Followed [this thread](https://github.com/ansible/ansible/issues/14426), which suggested the following ways:

    + Add `transport=paramiko` to **~/.ansible.cfg** file. ==> This fixed the issue.
      *Paramiko is a Python (2.6+, 3.3+) implementation of the SSHv2 protocol*
    + Add `sudo: yes` to  playbook, in the same place configuring **remote_user**.

## How to avoid host key check when connect to remote machine?

>Add the line `host_key_checking = False` to *~/.ansible.cfg* file.
or, just `export ANSIBLE_HOST_KEY_CHECKING=False`

## How to pass user/pass pair when calling ansible?

You can add following section to your inventory file:

``` ini
[all:vars]
ansible_connection=ssh 
ansible_ssh_user=vagrant 
ansible_ssh_pass=vagrant
```

## Use _--ask-pass_ option on ansible-playbook to ask for a password for ssh user

Like this: `ansible-playbook -i allhosts  test.yml --ask-pass`

Alternatively, you can use a password file to avoid inputting password:

    + Add the following line to _/etc/ansible/ansible.cfg_ or ~/.ansible.cfg: `vault_password_file = ~/.vault_pass`
    + `echo "password" > ~/.vault_pass`
    + `chmod 600 ~/.vault_pass`

## How to init a role __scaffolding__?

Use `ansible-galaxy init` command like this: `ansible-galaxy init -p roles nginx`

## How to collect remote machine _facts_?

Use **setup** module like this: `ansiable -m setup www`

## How to use Valut to protect your password?

First, create a var file with vault, like this:`ansible-vault create vars/main.yml`, edit with:`ansible-vault edit vars/main.yml`, and at deployment time: `ansible-playbook --ask-vault-pass site.yml`

## [How to disable host key checking?](http://docs.ansible.com/ansible/intro_getting_started.html#remote-connection-information)

By editing */etc/ansible/ansible.cfg* or *~/.ansible.cfg*:

```ini
[defaults]
host_key_checking = False
```

Alternatively this can be set by an environment variable:`export ANSIBLE_HOST_KEY_CHECKING=False`

## [Become (Privilege Escalation)](http://docs.ansible.com/ansible/become.html)

>Directives: _become_, _become_user_, _become_method_
Connection __variables__: _ansible_become_, _ansible_become_method_, _ansible_become_user_, _ansible_become_pass_
Command line option: _--ask-become-pass, -K_, _--become, -b_, _--become-user=BECOME_USER_. Like this:`ansible all -m ping -K`

# Reference

## [How to Install Ansible on CentOS 7 via yum](http://www.liquidweb.com/kb/how-to-install-ansible-on-centos-7-via-yum/)
## [An Ansible Tutorial](https://serversforhackers.com/an-ansible-tutorial)
## [Primer on Jinja Templating](https://github.com/mjhea0/thinkful-mentor/tree/master/python/jinja)
## [Playbook Conditional](http://docs.ansible.com/ansible/playbooks_conditionals.html)
  - *A number of Jinja2 “filters” can also be used in when statements, some of which are unique and provided by Ansible.*
  - *Combining when with with_items (see Loops), be aware that the when statement is processed separately for each item*
  - *the registered variable’s string contents are accessible with the ‘stdout’ value.*
## [jinja2 filters](http://docs.ansible.com/ansible/playbooks_filters.html)
## [Jinja Template Designer Documentation](http://jinja.pocoo.org/docs/dev/templates/#builtin-filters)
## [ansible quick reference](https://github.com/lorin/ansible-quickref)
