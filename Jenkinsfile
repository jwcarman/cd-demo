pipeline {
    agent {
        docker {
            image 'maven:3.6.1'
            args '-v maven-repo:/root/.m2 -u root'
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
                withSonarQubeEnv('Sonar') {
                    sh 'mvn -B sonar:sonar'
                }
            }
        }
        stage('Release') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'c7698903-f62f-4454-9942-64759a775633', passwordVariable: 'gh_password', usernameVariable: 'gh_username')]) {
                    configFileProvider([configFile(fileId: 'maven-settings', variable: 'M2_SETTINGS')]) {
                        sh "mvn -B -s ${M2_SETTINGS} release:prepare release:perform -Dusername=${gh_username} -Dpassword=${gh_password} -Darguments=-Dmaven.test.skip=true"
                    }

                }
            }
        }
    }
}
