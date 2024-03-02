# Manual Installation of jenkins

### Long Term Support release 

```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

### Weekly release

```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

The package installation will:

- Setup Jenkins as a daemon launched on start. Run systemctl cat jenkins for more details.

- Create a ‘jenkins’ user to run this service.

- Direct console log output to systemd-journald. Run journalctl -u jenkins.service if you are troubleshooting Jenkins.

- Populate /lib/systemd/system/jenkins.service with configuration parameters for the launch, e.g JENKINS_HOME

- Set Jenkins to listen on port 8080. Access this port with your browser to start configuration.

> If Jenkins fails to start because a port is in use, run systemctl edit jenkins and add the following:

```bash
[Service]
Environment="JENKINS_PORT=8081"
```

### Installation of Java

```bash
sudo apt update
sudo apt install fontconfig openjdk-17-jre

java -version
openjdk version "17.0.8" 2023-07-18
OpenJDK Runtime Environment (build 17.0.8+7-Debian-1deb12u1)
OpenJDK 64-Bit Server VM (build 17.0.8+7-Debian-1deb12u1, mixed mode, sharing)
```

### start Jenkins

```bash
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```

> If you have a firewall installed, you must add Jenkins as an exception. You must change YOURPORT in the script below to the port you want to use. Port 8080 is the most common.

```bash
YOURPORT=8080
PERM="--permanent"
SERV="$PERM --service=jenkins"

firewall-cmd $PERM --new-service=jenkins
firewall-cmd $SERV --set-short="Jenkins ports"
firewall-cmd $SERV --set-description="Jenkins port exceptions"
firewall-cmd $SERV --add-port=$YOURPORT/tcp
firewall-cmd $PERM --add-service=jenkins
firewall-cmd --zone=public --add-service=http --permanent
firewall-cmd --reload
```

ref : https://www.jenkins.io/doc/book/installing/linux/