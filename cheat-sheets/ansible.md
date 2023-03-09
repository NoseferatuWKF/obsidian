run playbook
```bash
ansible-playbook --ask-vault-pass -t tag --skip-tags skip playbook.yml
```

run playbook on remote git repo
```bash
ansible-pull -U git@github.com:userA/ansible.git --vault-pass-file password-file -t tag --skip-tags skip playbook.yml
```