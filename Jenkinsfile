pipeline {
    agent any

    environment {
        CI = 'true' // Requerido por Cypress
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    try {
                        sh '''
                        # Instala Node.js si no está disponible
                        if ! [ -x "$(command -v node)" ]; then
                            curl -fsSL https://deb.nodesource.com/setup_18.x | bash -
                            apt-get install -y nodejs
                        fi

                        # Instala dependencias
                        npm install
                        '''
                    } catch (Exception e) {
                        echo 'Error al instalar dependencias'
                        error 'Abandonando pipeline.'
                    }
                }
            }
        }

        stage('Run Cypress Tests') {
            steps {
                sh 'npx cypress run --browser chrome'
            }
        }

        stage('Archive Results') {
            steps {
                archiveArtifacts artifacts: 'cypress/videos/**/*, cypress/screenshots/**/*', allowEmptyArchive: true
                junit 'cypress/results/*.xml'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
