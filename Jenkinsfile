pipeline {
    agent {
        docker {
            image 'maven:3.6.1'
        }
    }
    stages {
        stage('Environment') {
            steps {
                sh "env"
            }
        }
        stage('Build') {
            steps {
                configFileProvider([configFile(fileId: 'maven-settings', variable: 'M2_SETTINGS')]) {
                    sh "mvn -B -s ${M2_SETTINGS} clean deploy"
                }
            }
        }
        stage('Sonar') {
            steps {
                configFileProvider([configFile(fileId: 'maven-settings', variable: 'M2_SETTINGS')]) {
                    withSonarQubeEnv('Sonar') {
                        sh 'mvn -B -s ${M2_SETTINGS} sonar:sonar'
                    }
                }
            }
        }
        stage('Release') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'gh_password', usernameVariable: 'gh_username')]) {
                    configFileProvider([configFile(fileId: 'maven-settings', variable: 'M2_SETTINGS')]) {
                        sh "mvn -B  -s ${M2_SETTINGS} release:prepare release:perform -Dusername=${gh_username} -Dpassword=${gh_password}"
                    }

                }
            }
        }
    }
}
