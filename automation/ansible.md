# [Ansible](https://docs.ansible.com/)

>python based

>push based automation

>Agent-less which makes it great for automating setting up local machine

[[cheat-sheets/ansible|cheat-sheets]]

# Tips
1. [include and import are different](https://docs.ansible.com/ansible/2.9/user_guide/playbooks_reuse_includes.html#includes-vs-imports)
2. [ansible magic variables](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_vars_facts.html#information-about-ansible-magic-variables)
3. [ansible builtin](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/index.html#plugin-index) (usually you don't need to install depends on the ansible version you have)
4. check [[yaml]] for advanced syntax

# Getting Started

create an inventory
```yaml
nodes: # group of managed nodes/remotes
	hosts: # put localhost for well... localhost
		remote:
			ansible_host: 192.168.1.100
		another_remote:
				ansible_host: somewhere.net # FQDN if you have
```

create a playbook
>import_tasks and include_tasks have different execution time
```yaml
- name: automate something
  hosts: nodes # inventory.hosts or put localhost for well... localhost
  remote_user: admin # the SSH user
  vars: # this can be encrypted
  pre_tasks: # this can be single, array, import_tasks, include_tasks
  tasks: # this can be single, array, import_tasks, include_tasks
  post_tasks: # this can be single, array, import_tasks, include_tasks

```

create a task
```yaml
# sudo apt install ripgrep
- name: install ripgrep
  become: true # sudo
  apt: name=ripgrep # can change it into an array to install multiple stuff
  tags: ripgrep # can also be an array
```

parallel tasks
```yaml
name: Run tasks in parallel
hosts: localhost
connection: local
gather_facts: no
tasks:
  - name: Pretend to create instances
    command: "sleep {{ item }}"  # Instead of calling a long running operation at a cloud provider, we just sleep.
    with_items:
      - 6
      - 8
      - 7
    register: _create_instances
    async: 600  # Maximum runtime in seconds. Adjust as needed.
    poll: 0  # Fire and continue (never poll)
	
  - name: Wait for creation to finish
    async_status:
      jid: "{{ item.ansible_job_id }}"
    register: _jobs
    until: _jobs.finished
    delay: 5  # Check every 5 seconds. Adjust as you like.
    retries: 10  # Retry up to 10 times. Adjust as needed.
    with_items: "{{ _create_instances.results }}"
```

# Working with secrets

encrypt/decrypt
>has a bunch of useful options to organize secrets
```bash
ansible-vault encrypt <file|stdin|var>
ansible-vault decrypt <file|stdin|var>
```