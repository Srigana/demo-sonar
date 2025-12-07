pipeline {
    agent any

    tools {
        maven 'Maven'    // Jenkins -> Manage Jenkins -> Tools
        jdk   'JDK17'    // Jenkins -> Manage Jenkins -> Tools
    }

    stages {

        stage('Checkout') {
            steps {
                // SCM checkout is handled automatically when using "Pipeline script from SCM"
                echo "Using repo from SCM configuration"
            }
        }

        stage('Build & Unit Tests') {
            steps {
                sh '''
                    mvn clean verify
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('My SonarQube') {   // Jenkins -> Configure System -> SonarQube servers
                    sh '''
                        mvn sonar:sonar \
                          -Dsonar.projectKey=demo-maven \
                          -Dsonar.token=$SONAR_AUTH_TOKEN
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    // Requires SonarQube webhook -> http://localhost:8080/sonarqube-webhook/
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo 'PIPELINE RESULT: SUCCESS (Build + Tests + Sonar + Quality Gate passed)'
        }
        failure {
            echo 'PIPELINE RESULT: FAILURE (Check Jenkins logs and SonarQube dashboard)'
        }
    }
}

