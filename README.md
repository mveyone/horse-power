# horse-power
3. Add Docker to Sudo group:
https://docs.docker.com/engine/install/linux-postinstall/

# (required) to be able to start minikube
$ sudo groupadd docker

# Add your user to the docker group, replace [user] with your username
$ sudo usermod -aG docker $USER

# To activate changes to the group
$ newgrp docker
-------------------------------------------------------------------------------------------------


------------------------------------------------------------------------------------
Steps to Deploy Website on EC2 Instence using Jenkins

Watch Jenkins and AWS شرح step by step on YouTube

Step 1:

Create AWS account & Sign In:

https://aws.amazon.com/

Step 2:

1. Create EC2 instence

2. Security Groups Rules:
    1. SSH
    2. HTTP: 80
    3. Custom rule: 8080
    4. Custom rule:5000

3. Create Key_Pair
    Download the .pem file

Step 3:

Change <file.pem> permissions

sudo chmod 400 file.pem

Step 4:

Connect to the created EC2 instence:

sudo ssh -i file.pem ubuntu@PUBLIC_IPv4_DNS_NAME

Step 5:

Install Jenkins:

sudo apt update -y

sudo apt-get install openjdk-8-jdk -y 

java -version

readlink -f $(which java)

JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java

echo $JAVA_HOME

export JAVA_HOME

sudo apt install wget 

wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
/etc/apt/sources.list.d/jenkins.list'

sudo apt update -y 

sudo apt-get install jenkins -y

Step 6:

Access Jenkins through browser:

  1. PUBLIC_IPv4_ADDRESS:8080

  2. sudo cat <jenkins_default_password_path>

  3. Fill in user details & Install suggested plugins

Step 7:

Create a job:

  1. Add Repository Link in Source Code Management

  2. Make sure the master branch name is the same as in the repository */main  or */master

  3. Add build setup then Execute Shell 

Commands:

cd Simple-Project/

ansible-playbook file.yml

Step 8:

Install Docker:

sudo apt-get update

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io

Step 9:

Add Docker to sudo group:

sudo groupadd docker

sudo usermod -aG docker $USER

newgrp docker 

sudo chmod 666 /var/run/docker.sock

Step 10:

Install Ansible:

sudo apt update

sudo apt install ansible -y

Step 11:

Run Job on Jenkins:

  1. Click on the job from the dashboard

  2. Build Now

Step 12:

Once the job run successfully:

Go to your browser: PUBLIC_IPv4_ADDRESS:5000


----------------------------------------
###################################################
#
#
#// This Jenkinsfile is to login with Docker Credentials and run ansible playbook file that will run docker commands to build image, run container and push image to dockerhub  

//Pipeline start here
pipeline {
    agent any

//Add Dockerhub Credentials
    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')
	}
  //Pipeline stages
    stages {
      // First Stage
        stage('First-Stage') {
          // Steps in first stage
            steps {
              // Command to login using dockerhub credentials  
              sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Second-Stage') {
            steps {
              // Run mutliple commands
              sh """
              cd ${APPLICATION_DIR}
              ansible-playbook playbook.yml
              """
            }
        }
    }
}


My first solutions was:

usermod -aG docker jenkins
usermod -aG root jenkins
chmod 664 /var/run/docker.sock

But none of them work for me, I tried:

chmod 777 /var/run/docker.sock

That works, but I don't know if it is the right call.



i used a pipeline with git and jenkins file a
 now i will try to use a freestyle pipeline
#
