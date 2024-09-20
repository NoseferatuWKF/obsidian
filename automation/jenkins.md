# [Jenkins](https://www.jenkins.io/)

>java based

>agents based
## Jenkins vs. Jenkins X

| Jenkins                                      | Jenkins X                                  |
| -------------------------------------------- | ------------------------------------------ |
| unopinionated and more flexible              | opinionated based on best practices        |
| bare bones requires plugins and integrations | simplified configuration                   |
| use GUI to do stuff                          | use CLI to do stuff, GUI to view pipelines |
| use with whatever you like or go naked       | Kubernetes native                          |

## Setting up

docker
```bash
docker run -d --name jenkins -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts-jdk11
```

docker compose
```yaml
services:
  jenkins:
    image: jenkins/jenkins:lts
    ports:
      - "8080:8080"
    volumes:
      - jenkins_home:/var/jenkins_home
  ssh-agent:
    image: jenkins/ssh-agent
volumes:
  jenkins_home:
```

init
```bash
# go to http://localhost:8080 and wait for jenkins to start up
docker exec -it jenkins bash
cat /var/jenkins_home/secrets/initialAdminPassword
# paste the password in the localhost client
# install recommended plugins or choose manually
# create first admin user
# set root url
```

## Distributed Builds Architecture

### Agent communications
1. The SSH connector
2. The inbound connector
	1. Manually/Java Web Start
	2. As a service
3. The inbound-HTTP connector/Headless HTTP
4. Custom script

### Creating fungible agents
1. Assume target agent does not always have the tools to carry on the job
2. Agents should be interchangeable and standardized for resource optimization

### Cloud Agents
1. [EC2 Plugin](https://plugins.jenkins.io/ec2)
2. [Azure VM Agents Plugin](https://plugins.jenkins.io/azure-vm-agents)
3. [JCloud Plugin](https://plugins.jenkins.io/jclouds-jenkins)

### Controller division strategies
1. By environment
2. By org chart
3. By product lines

## Controller Isolation

### Not building on the built-in node
1. Manage Jenkins
2. Select Built-In Node
3. Select Configure
4. Set the number of executors to 0
5. Make sure to also set up clouds or build agents to run builds on, otherwise builds won't be able to start
6. Consider using [Job Restrictions Plugin](https://plugins.jenkins.io/job-restrictions)
7. Save

### Security
1. Agent > Controller Security > Enable Agent -> Controller Access Control

## Using Jenkins agents

### Credentials
1. Generate ssh keypair
2. Go to Jenkins dashboard
3. Manage Jenkins > Credentials > System > Global credentials (unrestricted)
4. Add Credentials
5. Fill in form
	1. Kind: SSH Username with private key
	2. Scope: Global
	3. id: unique identifier (i.e; jenkins)
	5. username: jenkins (agent remote root directory)
	6. Private Key: Enter directly

### Setup agent
1. Make sure pubkey is in agent can be set on startup using JENKINS_AGENT_SSH_PUBKEY env
2. Go to Jenkins dashboard
3. Manage Jenkins > Manage Nodes and clouds
4. New Node
5. Fill the Node/agent name and select the type
6. Fill the fields
	3. Number of executors: 3 (concurrent builds)
	4. Remote root directory: /home/jenkins
	6. usage: only build jobs with label expression
	7. Launch method: Launch agents by SSH
		1. Host: ip address
		2. Credentials: jenkins (should be same as id)
		3. Host Key verification Strategy: Manually trusted key verification
7. Save
8. Launch agent

### Testing agent
1. Go to Jenkins dashboard
2. Select New Item
3. Enter a name
4. Select Freestyle project
5. Press OK
6. Check: Restrict where this project can be run
7. Key in label
8. Select Execute shell in Build Steps
9. echo $NODE_NAME
10. Save
11. Build Now
12. Click on Job dropdown in Build History -> Console Output

## Creating a pipeline
1. Go to Jenkins dashboard
2. Click on ` + New Item ` on the left side bar
3. Choose pipeline

## Github integration
1. Go to General > Pipeline > Definition
2. Choose Pipeline script from SCM
3. Choose Git at the SCM and fill out all the other good stuff
4. Make sure the script path is set correctly

## Enabling ssh
1. First generate a ssh keypair and add it to the ssh-agent [[core|ssh-add]]
2. Do a test connection with `ssh -T git@github.com` to add github to known hosts or any other way is fine as well
3. Go to github repo and add click on settings and add a deploy key which is the pubkey
4. Now go to Jenkins dashboard and go to Manage Jenkins > Manage Credentials
5. Add a new credential
6. In the `kind` dropdown SSH Username with private key
7. The ID should be github username
8. Enable `Enter directly` on the `Private Key` section
9. Paste the private key