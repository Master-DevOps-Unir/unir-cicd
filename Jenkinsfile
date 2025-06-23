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
                //junit 'results/unit/*_result.xml'
            }
        }

        // stage('API tests') {
        //     steps {
        //         echo 'Running API tests...'
        //         sh 'make test-api'
        //         archiveArtifacts artifacts: 'results/api/*.xml'
        //         junit 'results/api/*.xml'
        //     }
        // }

        // stage('E2E tests') {
        //     steps {
        //         echo 'Running E2E tests...'
        //         sh 'make test-e2e'
        //         archiveArtifacts artifacts: 'results/e2e/**', allowEmptyArchive: true
        //         junit 'results/e2e/*.xml'
        //     }
        // }
    }
    post {
        always {
            junit 'results/*_result.xml'
            cleanWs()
        }
    }
}