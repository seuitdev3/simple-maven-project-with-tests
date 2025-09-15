pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test -Dmaven.test.failure.ignore=true' // Continue even if tests fail
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml' // Still capture test results
                }
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package -DskipTests' // Skip tests during packaging
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
        unstable {
            echo 'Build is unstable due to test failures, but artifact was created'
        }
    }
}
