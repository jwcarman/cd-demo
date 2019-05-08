pipeline {
    agent {
        docker {
            image 'maven:3.6.1'
        }
    }
    stages {
        stage('Build') {
            steps {
                withSonarQubeEnv('Sonar') {
                    sh 'mvn -B clean install sonar:sonar'
                }

            }
        }
        stage('Release') {
            steps {

                withCredentials([usernamePassword(credentialsId: 'c7698903-f62f-4454-9942-64759a775633', passwordVariable: 'gh_password', usernameVariable: 'gh_username')]) {
                    configFileProvider([configFile(fileId: 'maven-settings', variable: 'M2_SETTINGS')]) {
                        sh "mvn -B -s ${M2_SETTINGS} release:prepare release:perform -Dusername=${gh_username} -Dpassword=${gh_password}"
                    }

                }
            }
        }
    }
}
