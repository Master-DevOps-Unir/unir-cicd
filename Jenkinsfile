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
                archiveArtifacts artifacts: 'results/**'
            }
        }

        stage('API tests') {
            steps {
                echo 'Running API tests...'
                sh 'make test-api'
                archiveArtifacts artifacts: 'results/**'
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

            // Simulación de envío de correo
            echo """
            Simulando envío de correo...
            
            
            
            
            Asunto: Resultado del Pipeline
            De: Jenkins Pipeline <jenkins@unir.net>
            Para: equipo@unir.net
            -----------
            Estimado equipo,
            El job '${env.JOB_NAME}' con número de ejecución #${env.BUILD_NUMBER} ha finalizado.
            Detalles:
            Fecha: ${new Date().format('dd/MM/yyyy HH:mm:ss')}
            Repositorio: ${env.GIT_URL}
            Rama: ${env.GIT_BRANCH}
            Commit: ${env.GIT_COMMIT}
            Autor: ${env.GIT_AUTHOR_NAME} <${env.GIT_AUTHOR_EMAIL}>
            Mensaje: ${env.GIT_COMMIT_MESSAGE}
            Resultado: ${currentBuild.currentResult}

            Estado: ${currentBuild.currentResult}
            URL: ${env.BUILD_URL}
            """
        }
    }
}