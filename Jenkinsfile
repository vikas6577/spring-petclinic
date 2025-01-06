pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://localhost:9000' // SonarQube URL
        SONAR_TOKEN = credentials('squ_5eea9621ee263ecdb894ca3cbeb0e2ead9dc8ff0') // Replace with your Jenkins credential ID
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/vikas6577/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                sh './gradlew build'
            }
        }

        stage('Run Tests') {
            steps {
                sh './gradlew test jacocoTestReport'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh './gradlew sonarqube -Dsonar.projectKey=spring-petclinic \
                                          -Dsonar.host.url=$SONAR_HOST_URL \
                                          -Dsonar.login=$SONAR_TOKEN'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 1, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }
    }
}
