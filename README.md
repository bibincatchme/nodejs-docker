# Project Title

Jenkins And Docker auto build 
## Getting Started

One developer full the code the Jenkins trigger and copy the code to Docker server.
Docker Build and Docker start take place in docker server

### Prerequisites


```
Jenkins Server with installed plugins
1.Git Plugin
2.Publish Over SSH

```

```
Docker Server
1.Install docker and test with a centos image
2.Create a user called dockeradmin
3.Add the id_rsa.pub key of jenkins user, under the dockeradmin of docker server and check the keyless authication to user dockeradmin (PubKeyAuthentication yes PermitRootLogin yes PasswordAuthentication yes)

```

Create a Pipeline Job
#General
GitHub Project
https://github.com/bibincatchme/nodejs-jenkinswithdocker/
Source Code Management
git 
https://github.com/bibincatchme/nodejs-jenkinswithdocker.git
BuildTrigger
GitHub hook trigger for GITScm polling (add jenkins server IP in git repo webhook (http://178.128.22.161:8080/github-webhook/))

Build
Execute Shell
#!/bin/bash
rsync -avz -e ssh /var/lib/jenkins/workspace/Devops-Node-test1/ dockeradmin@142.93.219.83:/opt/node



#!/bin/sh
ssh dockeradmin@142.93.219.83 <<EOF
docker stop nodejs 
docker rm nodejs
docker rmi node-web-app
cd /opt/node 
docker build -t node-web-app .
docker run --name nodejs  -p 3001:3000 -d  node-web-app

exit
EOF












============================================
PermitRootLogin yes
#StrictModes yes
#MaxAuthTries 6
#MaxSessions 10

PubkeyAuthentication yes

# Expect .ssh/authorized_keys2 to be disregarded by default in future.
#AuthorizedKeysFile     .ssh/authorized_keys .ssh/authorized_keys2

#AuthorizedPrincipalsFile none

#AuthorizedKeysCommand none
#AuthorizedKeysCommandUser nobody

# For this to work you will also need host keys in /etc/ssh/ssh_known_hosts
#HostbasedAuthentication no
# Change to yes if you don't trust ~/.ssh/known_hosts for
# HostbasedAuthentication
#IgnoreUserKnownHosts no
# Don't read the user's ~/.rhosts and ~/.shosts files
#IgnoreRhosts yes

# To disable tunneled clear text passwords, change to no here!
PasswordAuthentication yes
====================================
