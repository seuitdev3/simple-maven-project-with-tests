pipeline {
    agent any
    tools {
        maven 'MyMavenTool'
        jdk 'JDK11'
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
                sh 'mvn test -Dmaven.test.failure.ignore=true' // Continue even if tests fail
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
        unstable {
            echo 'Build is unstable due to test failures, but artifact was created'
        }
    }
}
