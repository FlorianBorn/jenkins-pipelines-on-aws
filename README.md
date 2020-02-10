# jenkins-pipelines-on-aws
Udacity Cloud DevOps Nanodegree / Use Jenkins to check and store a simple html file to AWS S3

# Steps
0. Project Setup
* in IAM create a new Group
* this group should have the following policies:
    - Amazon EC2 Full Access
    - Amazon S3 Full Access
* create a new user, who as has console and programmatic access
    - add this user to group created before 
1. Prepare AWS Ressouces
* create a new key-pair to be able connect via SSH to the EC2 instance, which will be created afterwards
  * In EC2 --> Key-Pairs --> Create key pair
  * copy this key to the machine from where the new EC2 instance will be accessed
  * set the permission of this key to read-only 
    chmod 400 my-shiny-new-key.pem
* start the EC2 instance
    * as key choose my-shiny-new-key
    * in Security Group allow Port 22 (SSH) and Port 8080 (Jenkins) for your own IP 
    * (an new security group can also be created after the instance is launched)  

2. Install Jenkins
    sudo apt update && apt upgrade && apt install -y default-jdk

    Add Jenkins' repo key to the system:
    wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -

    Append the Debian package repo address to the server's sources.list:
    sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

    Run update again and finally install jenkins:
    sudo apt update
    sudo apt install jenkins

    Check if Jenkins is running:
    sudo systemctl status jenkins (service --status-all should also work)

    If Jenkins is up and running, it can be accessed on its default port 8080. The fist screen one should see, will look like this:

3. Set Up Jenkins
    To get the initial admin password, run the following command inside the ssh terminal, which is connected to the ec2 instance running jenkins:
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword

    Install the suggested plugins, create an admin user,set the Jenkins URL and finish.

    Install Plugins:
    - Blue Ocean
    - Pipeline: AWS Steps

    After the plugins were successfully installed, restart jenkins

4. Set Up a Pipeline
    Create a git Repository (like this)
    Within this repository create a html-File like this:
    '''
        <!doctype html>
        <html>
            <head>
                <title>Static HTML Site</title>
            </head>
            <body>
                <p>This is the first paragraph in a simple  Static HTML site. There is no <strong>CSS</  strong> or <strong>JavaScript</script>.</p>
            </body>
        </html>        
    '''
    Create a Jenkinsfile containing the pipeline:
    '''
        pipeline {
            agent any
            stages {
                stage('Build') {
                    steps {
                        sh "echo Hello World"
                        sh '''
                            echo "Multiline shell steps works too"
                            ls -lah
                        '''
                    }
                }
            }
        }    
    '''

    In Jenkins, select the Blue Ocean Plugin and create a Pipeline.
    Select GitHub as SCM.
    In order to access GitHub, Jenkins ask for an access token. Create this token and connect Jenkins and GitHub.