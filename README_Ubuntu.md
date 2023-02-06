# Install Jenkins on AWS EC2
Jenkins is a self-contained Java-based program, ready to run out-of-the-box, with packages for Windows, Mac OS X and other Unix-like operating systems. As an extensible automation server, Jenkins can be used as a simple CI server or turned into the continuous delivery hub for any project.



### Prerequisites
1. EC2 Ubuntu Instance 
   - With Internet Access
   - Security Group with Port `8080` open for internet

1. Java v1.8.x 

## Install Java
Update the OS

```sudo apt update -y```

We will be using open java for our demo, Get latest version from http://openjdk.java.net/install/
```sh
sudo apt install openjdk-11-jre
```

### Confirm Java Version
Lets install java and set the java home
```sh
java -version
#find java path
find /usr  openjdk -type l 2>/dev/null | grep '^/usr/lib/jvm/java' | grep openjdk-amd64$
#assign the java path to an env var
javahome=`find /usr  openjdk -type l 2>/dev/null | grep '^/usr/lib/jvm/java' | grep openjdk-amd64$`
#Add JAVA_HOME and update of PATH to ~/.bash_profile
#example value set below
echo "export JAVA_HOME=$javahome" >> ~/.bashrc
echo 'PATH=$PATH:$JAVA_HOME' >> ~/.bashrc
echo 'export PATH' >> ~/.bashrc
chmod 755 ~/.bashrc
```
### To set it permanently update your .bashrc

```sh
. ~/.bashrc
```

```
[root@~]# java -version
openjdk version "1.8.0_151"
OpenJDK Runtime Environment (build 1.8.0_151-b12)
OpenJDK 64-Bit Server VM (build 25.151-b12, mixed mode)
```

## Install Jenkins

This is as per the instructions on https://www.jenkins.io/doc/book/installing/linux/

```sh
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

### Start Jenkins
```sh
# Start jenkins service
sudo systemctl start jenkins

# Setup Jenkins to start at boot,
sudo systemctl enable jenkins
```

#### Accessing Jenkins
By default jenkins runs at port `8080`, You can access jenkins at
```sh
http://YOUR-SERVER-PUBLIC-IP:8080
```
#### Configure Jenkins
- The default Username is `admin`
- Grab the default password 
  - Password Location:`/var/lib/jenkins/secrets/initialAdminPassword`
- `Skip` Plugin Installation; _We can do it later_
- Change admin password
  - `Admin` > `Configure` > `Password`
- Configure `java` path
  - `Dashboard` > `Manage Jenkins` > `Global Tool Configuration` > `JDK`  > Enter some name like Java1.8 > Enter JAVA_HOME from the server > Scroll to bottom > `Save`
- `Dashboard` > `Manage Jenkins` > `Manage Users` > `Create User` > Create another admin user id

### Test Jenkins Jobs
1. `Dashboard` > `New item`
1. Enter an item name – `My-First-Project`
   - Chose `Freestyle` project
1. Under Build section
	Execute shell : echo "Welcome to Jenkins Demo"
1. Save your job 
1. Click on Build Now
2. Click on latest build link in Build History section
3. Click on "Console Output"

### Next Step

