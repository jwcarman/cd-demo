pipeline {
    agent {
        docker {
            image 'maven:3.6.1'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Release') {
            steps {
                sh 'mvn release:prepare release:perform'
            }
        }
    }
}
