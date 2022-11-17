pipeline {
    agent any

    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')
    }
  stages {

        stage ('docker hub credentials')
        steps{
          sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
        }
        stage('Build & push Dockerfile') {
            steps {
                sh "ansible-playbook ansible-playbook.yml"
            }
        }
        
    }
}