pipeline {
    agent any

    tools {
        maven 'Maven'   // must match Jenkins Tools config
        jdk   'JDK17'
    }

    stages {
        stage('Checkout') {
            steps {
                // Jenkins will re-checkout anyway when using SCM,
                // but this keeps the script valid if run inline.
                echo 'Using repo from SCM configuration'
            }
        }

        stage('Build & SonarQube') {
            steps {
                withSonarQubeEnv('My SonarQube') { // name from Configure System
                    sh '''
                        mvn clean verify sonar:sonar \
                          -Dsonar.projectKey=demo-maven
                    '''
                }
            }
        }
    }
}

