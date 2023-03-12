## [Ansible](https://docs.ansible.com/)
*push based automation*

>Great for automating setting up local machine

[[cheat-sheets/ansible|cheat-sheets]]

### Installing on Ubuntu
```bash
sudo apt update && /
sudo apt install software-properties-common && /
sudo add-apt-repository -y --update ppa:ansible/ansible && /
sudo apt install ansible
```

### Tips
1. [include and import are different](https://docs.ansible.com/ansible/2.9/user_guide/playbooks_reuse_includes.html#includes-vs-imports)
2. [ansible magic variables](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_vars_facts.html#information-about-ansible-magic-variables)
3. [ansible builtin](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/index.html#plugin-index) (usually you don't need to install depends on the ansible version you have)
4. check [[yaml]] for advanced syntax

### How to
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