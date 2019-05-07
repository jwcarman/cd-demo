pipeline {
    agent {
        docker {
            image 'maven:3.6.1'
            args '-u root'
        }
    }
    stages {
        stage('Identify') {
            steps {
                sh 'id'
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
