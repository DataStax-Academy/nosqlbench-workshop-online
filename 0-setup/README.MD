
![OK](https://github.com/DataStax-Academy/nosqlbech-workshop-online/blob/master/materials/images/title-page.png?raw=true)

# If you are using Katacoda
If you wish to use our Katacoda scenarios (**recommended**) then go here -> https://katacoda.com/shallada/courses/nosqlbench-intro

# If you wish to do things on your own machine
For those of you who would like to have a basic setup working on your own machine follow the steps below. **If you are using Katacoda you DO NOT NEED THESE STEPS**.

## Install Docker and Download NOSQLBench
**NOTE:** Docker is **NOT** a pre-requisite for NoSQLBench, but is used in the following instructions to quickly spin up an Apache Cassandra™ cluster for use with NoSQLBench in our scenarios.


### 1. Install Docker

Docker is an open-source project that automates the deployment of software applications inside containers by providing an additional layer of abstraction and automation of OS-level virtualization on Linux.

![Windows](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/windows32.png?raw=true) : To install on **windows** please use the following installer [Docker Dekstop for Windows Installer](https://download.docker.com/win/stable/Docker%20Desktop%20Installer.exe)

![osx](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/mac32.png?raw=true) : To install on **MAC OS**  use [Docker Dekstop for MAC Installer](https://download.docker.com/mac/stable/Docker.dmg) or [homebrew](https://docs.brew.sh/Installation) with following commands:
```bash
# Fetch latest version of homebrew and formula.
brew update              
# Tap the Caskroom/Cask repository from Github using HTTPS.
brew tap caskroom/cask                
# Searches all known Casks for a partial or exact match.
brew search docker                    
# Displays information about the given Cask
brew cask info docker
# Install the given cask.
brew cask install docker              
# Remove any older versions from the cellar.
brew cleanup
# Validate installation
docker -v
```

![linux](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/linux32.png?raw=true) : To install on linux (centOS) you can use the following commands
```bash
# Remove if already install
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
# Utils
sudo yum install -y yum-utils

# Add docker-ce repo
sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
# Install
sudo dnf -y  install docker-ce --nobest
# Enable service
sudo systemctl enable --now docker
# Get Status
systemctl status  docker

# Logout....Login
exit
# Create user
sudo usermod -aG docker $USER
newgrp docker

# Validation
docker images
docker run hello-world
docker -v
```
### 2. Download NoSQLBench

You can run NoSQLBench via a Linux binary or jar file. It is recommended to use the binary as it includes its own JVM and does not need to manage Java downloads, but for those NOT using Linux systems the jar is fine. If you are using the jar Java 14 is recommended.

![Windows](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/windows32.png?raw=true)![osx](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/mac32.png?raw=true): To download and install on Windows or OSX simply download the latest release jar file [HERE](https://github.com/nosqlbench/nosqlbench/releases/latest/download/nb.jar)

![linux](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/linux32.png?raw=true) : To install on linux you can use the following commands
```
wget https://github.com/nosqlbench/nosqlbench/releases/latest/download/nb
```
