pipeline {
    agent any

    // 1. Opciones del Job
    options {
        disableConcurrentBuilds()
        timestamps()
        timeout(time: 5, unit: 'MINUTES')
    }

    // 2. Variables de entorno
    environment {
        FORCE_COLOR = '0'
        NO_COLOR   = 'true'
    }

    stages {
        // 3. Auditoría de herramientas - Incluye etapa 'Audit tools' imprime la versión
        stage('Audit tools') {
            steps {
                sh 'node --version'
            }
        }

        // 4. Instalación de dependencias - Ejecuta npm install
        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }

        // 5. Comprobación del formato
        stage('Format check') {
            steps {
                sh 'npm run format:check'
            }
        }

        // 6. Chequeo de calidad de código
        stage('Code quality') {
            steps {
                sh 'npm run lint'
            }
        }

        // 7. Comprobar tipos / type-ckeck
        stage('Type check') {
            steps {
                sh 'npm run type-check'
            }
        }

        // 8. Ejecución de tests
        stage('Tests') {
            steps {
                sh 'npm run test'
            }
        }

        // 9. Construcción y archivado: build y archivar dist
        stage('Build') {
            steps {
                sh 'npm run build'
                archiveArtifacts artifacts: 'dist/**', fingerprint: true
            }
        }

    }

    // 10. Etapas finales: mensajes y limpieza del ws.
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Review logs.'
        }
        always {
            cleanWs()
        }
    }
}