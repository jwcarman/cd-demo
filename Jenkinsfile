pipeline {
    agent {
        docker {
            image 'maven:3.6.1'
            args '-v $HOME/.m2:/root/.m2 -v /etc/passwd:/etc/passwd'
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
