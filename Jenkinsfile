pipeline {
    agent any

    tools {
        maven 'Maven'    // must match your Maven tool name in Jenkins
        jdk   'JDK17'    // adjust if your JDK tool has a different name
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Using repo from SCM configuration'
            }
        }

        stage('Build & SonarQube') {
            steps {
                withSonarQubeEnv('My SonarQube') {  // name from Configure System
                    sh '''
                        mvn clean verify sonar:sonar \
                          -Dsonar.projectKey=demo-maven \
                          -Dsonar.token=$SONAR_AUTH_TOKEN
                    '''
                }
            }
        }
    }
}
