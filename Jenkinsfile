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
                // archiveArtifacts artifacts: 'results/*.xml'
                sh '''
                    mkdir -p results/unit
                    mv results/unit_result.xml results/unit/
                    mv results/coverage.xml results/unit/ || true
                    mv results/coverage results/unit/ || true
                '''
                archiveArtifacts artifacts: 'results/unit/**'
            }
        }

        stage('API tests') {
            steps {
                echo 'Running API tests...'
                sh 'make test-api'
                // archiveArtifacts artifacts: 'results/api_result.xml'
                sh '''
                    mkdir -p results/e2e
                    mv results/cypress results/e2e/ || true
                    mv results/*.json results/e2e/ || true
                '''
                archiveArtifacts artifacts: 'results/e2e/**', allowEmptyArchive: true
            }
        }

        // stage('E2E tests') {
        //     steps {
        //         echo 'Running E2E tests...'
        //         sh 'make test-e2e'
        //         archiveArtifacts artifacts: 'results/**', allowEmptyArchive: true
        //     }
        // }
    }
    post {
        always {
            // junit 'results/*_result.xml'
            junit 'results/**/**/*.xml' 
            cleanWs()
        }
    }
}