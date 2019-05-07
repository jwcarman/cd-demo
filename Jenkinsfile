pipeline {
    agent {
        docker {
            image 'maven:3.6.1'
            args '-u root'
        }
    }
    stages {
        stage('GitHub Key') {
            steps {
                sh 'ssh-keyscan -t rsa github.com >> ~/.ssh/known_hosts'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B clean install'
            }
        }
        stage('Release') {
            steps {
                sshagent(['jenkins']) {
                    sh "mvn -B release:prepare release:perform"
                }
            }
        }
    }
}
