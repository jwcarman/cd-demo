pipeline {
    agent {
        docker {
            image 'maven:3.6.1'
            args "-e HOME=${JENKINS_HOME}"
        }
    }
    stages {
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
