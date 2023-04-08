# [Puppet](https://www.puppet.com/)

>ruby based

>pull based automation

>prefer to use ansible to setup up local machine as it has faster support for different OS. https://www.puppet.com/docs/bolt/latest/bolt_installing.html

## Bolt

>puppet agentless solution

>it has a cool ASCII art

### Getting started

installation
```bash
wget https://apt.puppet.com/puppet-tools-release-$(lsb_release -cs).deb
sudo dpkg -i puppet-tools-release-bionic.deb
sudo apt-get update 
sudo apt-get install puppet-bolt
```

run command 
```bash
bolt command run whoami -t localhost
bolt command run whoami -t <target|group|all> # to use inventory.yaml
```

create a project
```bash
bolt project init <project>
```

add a module
>somehow cannot remove a module using the cli
```bash
bolt module add puppetlabs/apt
```

create a plan
```bash
bolt plan new <project>::<module>::<plan> # yaml
bolt plan new <project>::<module>::<plan> --pp # pp

# then you can do
bolt plan show
```

run plan
```bash
bolt plan run <project>::<module>::<plan> -t <target>
```

### Managing secrets

create new pem key
```bash
bolt secret createkeys -t <target>
```

ecrypt/decrypt
>mostly sucks, decrypt can be used in yaml file, but at this point why not just use sops. decrypt cannot accept stdin
```bash
bolt secret encrypt <plaintext>
bolt secret decrypt <ecrypted-value>
```

configuration
```yaml
plugins:
  pkcs7:
    keysize: 4096
    private_key: ./nothing_to_see_here/private.pkcs7.pem
    public_key: ./nothing_to_see_here/public.pkcs7.pem
```

usage
```yaml
targets:
  - uri: example.com
    config:
      ssh:
        password:
          _plugin: pkcs7
          encrypted_value: |
            ENC[PKCS7,MY_ENCRYPTED_DATA]
```

### Writing a plan

[Plan Structure](https://www.puppet.com/docs/bolt/latest/writing_yaml_plans.html#plan-structure)

plan.yaml
```yaml
# This is the structure of a simple plan. To learn more about writing
# YAML plans, see the documentation: http://pup.pt/bolt-yaml-plans

# The description sets the description of the plan that will appear
# in 'bolt plan show' output.
description: A plan created with bolt plan new

# The parameters key defines the parameters that can be passed to
# the plan.
parameters:
  targets:
    type: TargetSpec
    description: A list of targets to run actions on
    default: localhost

# The steps key defines the actions the plan will take in order.
steps:
  - message: Hello from my_project::zsh::utils
  - name: command_step
    command: whoami
    targets: $targets

# The return key sets the return value of the plan.
return: $command_step
```

for the pp lovers
```pp
# This is the structure of a simple plan. To learn more about writing
# Puppet plans, see the documentation: http://pup.pt/bolt-puppet-plans

# The summary sets the description of the plan that will appear
# in 'bolt plan show' output. Bolt uses puppet-strings to parse the
# summary and parameters from the plan.
# @summary A plan created with bolt plan new.
# @param targets The targets to run on.
plan my_project::zsh::utils (
  TargetSpec $targets = "localhost"
) {
  out::message("Hello from my_project::zsh::utils")
  $command_result = run_command('whoami', $targets)
  return $command_result
}
```
