pipeline {
    agent any

  stages {
        stage('Build & push Dockerfile') {
            steps {
                sh "ansible-playbook ansible-playbook.yml"
            }
        }
        
    }
}