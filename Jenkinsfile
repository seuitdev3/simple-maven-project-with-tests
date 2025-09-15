pipeline {
    agent any
    tools {
        maven 'M3'  // Requires Maven tool configured in Jenkins
        jdk 'JDK11' // Requires JDK tool configured in Jenkins
    }
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
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml' // Archive test results
                }
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package -DskipTests' // Skip tests since they already ran
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            cleanWs() // Clean workspace after build
        }
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed! Check the logs for details.'
        }
    }
}
