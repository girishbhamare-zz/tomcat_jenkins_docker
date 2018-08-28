# tomcat_jenkins_docker
Dockerize a Web Application and added Jenkins support


    Project is : Deploy sample.war file on tomcat and dispay web page "hello"
    i.e Install Tomcat as base image
        Add sample.war and server.xml into it.
        From Jenkins , it builds the image and run that images as a container.
        

**Prerequisite :
Need Ec2 (Ubuntu 16) Machine:
Install Java 1.8

# Install Docker: 
    1.sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
    2  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    3  sudo apt-key fingerprint 0EBFCD88
    4  sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    5  sudo apt-get update
    6  sudo apt-get install docker-ce
    7  docker -v
# Install Java:
    sudo apt-add-repository ppa:webupd8team/java
    sudo apt-get update
    sudo apt-get install oracle-java8-installer
     java -version

# Jenkins Instllation:
 ### Step 1:
1.First, we'll add the repository key to the system.
  $ wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -

    When the key is added, the system will return OK. Next, we'll append the Debian package repository address to the server's sources.list:

  $ echo deb https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list
  
    When both of these are in place, we'll run update so that apt-get will use the new repository:

  $sudo apt-get update
  
    Finally, we'll install Jenkins and its dependencies, including Java:

$sudo apt-get install jenkins
    
    Now that Jenkins and its dependencies are in place, we'll start the Jenkins server.
    
  ### Step 2 — Starting Jenkins
    Using systemctl we'll start Jenkins:
    
$sudo systemctl start jenkins  
   
    Since systemctl doesn't display output, we'll use its status command to verify that it started successfully:

 $sudo systemctl status jenkins
    
    If everything went well, the beginning of the output should show that the service is active and configured to start at boot:

    Output
    ● jenkins.service - LSB: Start Jenkins at boot time
      Loaded: loaded (/etc/init.d/jenkins; bad; vendor preset: enabled)
      Active:active (exited) since Thu 2017-04-20 16:51:13 UTC; 2min 7s ago
        Docs: man:systemd-sysv-generator(8)
    Now that Jenkins is running, we'll adjust our firewall rules so that we can reach Jenkins from a web browser to complete the initial set up.
--------------------------
### Step 3 — Opening the Firewall

    By default, Jenkins runs on port 8080, so we'll open that port using ufw:

$sudo ufw allow 8080

        We can see the new rules by checking UFW's status.

$sudo ufw status

    We should see that traffic is allowed to port 8080 from anywhere:

    Output
    Status: active
    
    To                         Action      From
    --                         ------      ----
    OpenSSH                    ALLOW       Anywhere
    8080                       ALLOW       Anywhere
    OpenSSH (v6)               ALLOW       Anywhere (v6)
    8080 (v6)                  ALLOW       Anywhere (v6)

---------------------------------------------------------------------------------------------------
### Step 4 — Setting up Jenkins:

    Copy public ip of ubuntu host and load in browser, it load jenkin page and ask for first time password
    http://ip_address_or_domain_name:8080

    **We should see "Unlock Jenkins" screen, which displays the location of the initial password

    **In the terminal window, we'll use the cat command to display the password:

$sudo cat /var/lib/jenkins/secrets/initialAdminPassword

    **We'll copy the 32-character alphanumeric password from the terminal and paste it into the "Administrator password" field, then click "Continue". The next screen presents the option of installing suggested plugins or selecting specific plugins.


    ***We'll click the "Install suggested plugins" option, which will immediately begin the installation process:
    
    **When the installation is complete, we'll be prompted to set up the first administrative user. It's possible to skip this step and continue as admin using the initial password we used above, but we'll take a moment to create the user.
    
    **Once the first admin user is in place, you should see a "Jenkins is ready!" confirmation screen.
    
    **Click "Start using Jenkins" to visit the main Jenkins dashboard:
    
    ****At this point, Jenkins has been successfully installed.
    
    -------------------------------------------------------------------------
    
## Configure Jenkins Server

1. Head over to Jenkins Dashboard –> Manage jenkins –> Manage Plugins.

2. Under available tab, search for “Docker Plugin” and install it.

3. Once installed, head over to jenkins Dashboard –> Manage jenkins –>Configure system.

4. Under “Configure System”, if you scroll down, there will be a section named “cloud” at the last. There you can fill out the docker host parameters.

5. Under docke,radd Docker Host URI = unix:///var/run/docker.sock
6. Save changes.


 ## Create a jenkin job 

Go to Jenkin -> new item -> enter a job name -> select free style project -> click on ok 
click on Created Job >> Configure
Click on checkbox "This project is parameterized"

    Name =IMAGE_NAME
    Value = "put any string which will display as docker image"
    
Go into Buld section 
**Execute shell

    cd "path of the folder where you cloned or kept Docker file"
    docker build -t ${IMAGE_NAME}:latest .
    docker run -i -t -d -p 9090:9090 ${IMAGE_NAME} 
    
    Save and Apply
    
---------------------------------------------------------------------------------------------------------------


Go to Ubuntu machine
Clone this project.
Go to /opt/tomcat_jenkins_docker/

### hit following commands


    $sudo chmod 666 server.xml
    $sudo chmod 666 /var/run/docker.sock    
    $sudo chmod 666 server.xml
    $usermod -aG docker jenkins
    
### Restart terminal.

## Go to jenikin and build a job.
### Console output shows.
    Building in workspace /var/lib/jenkins/workspace/tomcat demo
    [tomcat demo] $ /bin/sh -xe /tmp/jenkins1256132220241435311.sh
    + cd /opt/tomcatdemo
    + docker build -t test:latest .
    Sending build context to Docker daemon  16.38kB
    
    Step 1/6 : FROM tomcat:8.0.43-jre8
     ---> 03fc9c4ee243
    Step 2/6 : ADD WEB-INF/lib/sample.war /usr/local/tomcat/webapps/
     ---> Using cache
     ---> 764c453cdc9d
    Step 3/6 : ADD server.xml /usr/local/tomcat/conf/
     ---> b3007b849e84
    Step 4/6 : EXPOSE 9090
     ---> Running in 95ba104553ba
    Removing intermediate container 95ba104553ba
     ---> 8c13d6816006
    Step 5/6 : CMD chmod +x /usr/local/tomcat/bin/catalina.sh
     ---> Running in 3795b725d48c
    Removing intermediate container 3795b725d48c
     ---> 495d62d754e6
    Step 6/6 : CMD ["catalina.sh", "run"]
     ---> Running in bc49f0f0a533
    Removing intermediate container bc49f0f0a533
     ---> b17d1d5ea6e3
    Successfully built b17d1d5ea6e3
    Successfully tagged test:latest
    + docker run -i -t -d -p 9090:9090 test
    b11c0366563db355c28f6065f10b3e2dfdeff75b5de169a79a240cd59d5afab9
    Finished: SUCCESS

### Go to browser  and enter http://ipaddress:9090/sample
### should show Sample "Hello, World" Application.
















    
    









    
    


