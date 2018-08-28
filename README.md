# tomcat_jenkins_docker
Dockerize a Web Application and added Jenkins support

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






    
    


