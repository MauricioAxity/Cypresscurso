pipeline {
    agent any

    environment {
        // Variable requerida por Cypress
        CI = 'true'
    }

    stages {
        stage('Checkout') {
            steps {
                // Clona el repositorio
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // Instala dependencias usando npm
                sh '''
                # Verifica si Node.js está instalado
                if ! [ -x "$(command -v node)" ]; then
                    curl -fsSL https://deb.nodesource.com/setup_18.x | bash -
                    apt-get install -y nodejs
                fi

                # Instala las dependencias del proyecto
                npm install
                '''
            }
        }

        stage('Run Cypress Tests') {
            steps {
                // Ejecuta las pruebas de Cypress
                sh 'npx cypress run --browser chrome'
            }
        }

        stage('Archive Results') {
            steps {
                // Archiva videos y capturas de pantalla generados por Cypress
                archiveArtifacts artifacts: 'cypress/videos/**/*, cypress/screenshots/**/*', allowEmptyArchive: true

                // Publica los resultados en formato JUnit
                junit 'cypress/results/*.xml'
            }
        }
    }

    post {
        always {
            // Limpia el workspace después de la ejecución
            cleanWs()
        }
    }
}
