pipeline {
    agent {
        docker {
            image 'maven:3.6.1'
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
                withCredentials([usernamePassword(credentialsId: 'c7698903-f62f-4454-9942-64759a775633', passwordVariable: 'gh_password', usernameVariable: 'gh_username')]) {
                    sh "mvn -B release:prepare release:perform -Dusername=${gh_username} -Dpassword=${gh_password}"
                }
            }
        }
    }
}
