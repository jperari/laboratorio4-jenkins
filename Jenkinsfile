pipeline {
    agent any

    // 1. Opciones del Job + Opcional - 2. Opciones del Job extra
    options {
        disableConcurrentBuilds()         // 1. Opciones del Job - Deshabilita buils concurrentes
        timestamps()                      // 1. Opciones del Job - Añadir marcas te tiempo
        timeout(time: 5, unit: 'MINUTES') // 1. Opciones del Job - Poner timeout
        buildDiscarder(logRotator(numToKeepStr: '10')) // Opcional - 2. Opciones del Job extra - Mantener 10 últimas ejecuciones
    }

    // 2. Variables de entorno - Heredadas en todas las etapas
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

        // Opcional 3 - Linting paralelo
        stage('Linting') {
            parallel {
                // 5. Comprobación del formato
                stage('Format check') {
                    steps {
                        sh 'npm run format:check'
                    }
                }
                // 6. Chequeo de calidad de código
                stage('Code quality') {
                    steps {
                        // Opcional 4 - Code quality permisivo - Añade warnError con mensaje y cambia etiqueta
                        warnError('No se superaron los chequeos de calidad de código.') {
                            sh 'npm run lint'
                        }
                        script {
                            if (currentBuild.result == 'UNSTABLE') {
                                currentBuild.description = 'UNSTABLE: Code quality'
                            }
                        }
                    }
                }
            }
        }

        // 7. Comprobar tipos / type-ckeck
        stage('Type check') {
            steps {
                sh 'npm run type-check'
            }
        }

        // 8. Ejecución de tests <-- La cambio debajo para añadir coverage
        //stage('Tests') {
        //    steps {
        //        sh 'npm run test'
        //    }
        //}

        // 8. Ejecución de tests + Opcional 1 test con cobertura y HTML Publisher
        stage('Tests') {
            steps {
                sh 'npm run test:coverage'
                
                // Hay que instalar el plugin en Jenkins, si no falla.
                publishHTML(target: [
                    reportDir: 'coverage',             // Directorio de resultados
                    reportFiles: 'index.html',         // Nombre del fichero de informe
                    reportName: 'Coverage Report'      // Nombre de los informes
                    keepAll: true,                     // Archiva históricos
                    alwaysLinkToLastBuild: true,       // Enlaza a la última ejecución
                    allowMissing: true,                // Permite fallos si no están los ficheros
                ])
            }
        }

        // 9. Construcción y archivado: build y archivar dist
        stage('Build') {
            steps {
                sh 'npm run build'
                archiveArtifacts artifacts: 'dist/**', fingerprint: true
            }
        }

        // Opcional 5. Pruebas end-to-end (E2E) con Docker Compose
        stage('E2E Tests') {
            environment {
                TEST_MODE = 'e2e'
            }
            steps {
                sh '''
                    docker compose -f compose.e2e.yml run tests
                '''
            }
            post {
                always {
                    sh 'docker compose -f compose.e2e.yml down -v --remove-orphans || true'
                }
            }
        }

        // Opcional 6. Construcción y publicación de imagen Docker
        // Pdte.
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