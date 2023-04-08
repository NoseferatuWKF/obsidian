# [Jenkins](https://www.jenkins.io/)
>run remote pipelines from localhost that other CI/CD tools can't do

## Jenkins vs. Jenkins X

| Jenkins | Jenkins X |
|-|-|
| unopinionated and more flexible | opinionated based on best practices |
| bare bones requires plugins and integrations | simplified configuration |
| use GUI to do stuff | use CLI to do stuff, GUI to view pipelines |
| use with whatever you like or go naked | Kubernetes native |


## Setting up

> technically you can follow the official [docs](https://www.jenkins.io/doc/book/installing/) but this is how I do it
```bash
docker pull jenkins/jenkins:lts-jdk11
docker run -d --name jenkins -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts-jdk11
# go to http://localhost:8080 and wait for jenkins to start up
docker exec -it jenkins bash
cat /var/jenkins_home/secrets/initialAdminPassword
# paste the password in the localhost client
# everything else should be straightforward
```

## Hello Jenkins

### Creating a pipeline
1. Go to Jenkins dashboard
2. Click on ` + New Item ` on the left side bar
3. Choose pipeline

### Github integration
1. Go to General > Pipeline > Definition
2. Choose Pipeline script from SCM
3. Choose Git at the SCM and fill out all the other good stuff
4. Make sure the script path is set correctly

### Enabling ssh
1. First generate a ssh keypair and add it to the ssh-agent [[linux | ssh-add]]
2. Do a test connection with `ssh -T git@github.com` to add github to known hosts or any other way is fine as well
3. Go to github repo and add click on settings and add a deploy key which is the pubkey
4. Now go to Jenkins dashboard and go to Manage Jenkins > Manage Credentials
5. Add a new credential
6. In the `kind` dropdown SSH Username with private key
7. The ID should be github username
8. Enable `Enter directly` on the `Private Key` section
9. Paste the private key

## Working with rust
>Just use something else
1. Install rust
2. Install build-essential (requires sudo)
sdsd
