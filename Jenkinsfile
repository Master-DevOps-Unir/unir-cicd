pipeline {
    agent {
        label 'agent-1'
    }
    stages {
        stage('Source') {
            steps {
                git 'https://github.com/Master-DevOps-Unir/unir-cicd.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building stage!'
                sh 'make build'
            }
        }
        stage('Unit tests') {
            steps {
                sh 'make test-unit'
                archiveArtifacts artifacts: 'results/unit/**'
            }
        }

        stage('API tests') {
            steps {
                echo 'Running API tests...'
                sh 'make test-api'
                archiveArtifacts artifacts: 'results/api_result.xml'
            }
        }

        stage('E2E tests') {
            steps {
                echo 'Running E2E tests...'
                sh 'make test-e2e'
                archiveArtifacts artifacts: 'results/**', allowEmptyArchive: true
            }
        }
    }
    post {
        always {
            // junit 'results/*_result.xml'
            junit 'results/**/**/*.xml' 
            cleanWs()
        }
    }
}