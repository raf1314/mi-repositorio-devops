pipeline {
    agent any

    stages {
        stage('Clonar repositorio') {
            steps {
                checkout scm
            }
        }

        stage('Instalar dependencias y pruebas') {
            steps {
                sh '''
                    pip install -r app/requirements.txt
                    pytest tests/
                '''
            }
        }

        stage('Build con Docker') {
            steps {
                sh 'docker build -t mi-app-flask ./app'
            }
        }

        stage('Deploy local con Docker') {
            steps {
                sh '''
                    docker stop mi-app-flask || true
                    docker rm mi-app-flask || true
                    docker run -d --name mi-app-flask -p 5000:5000 mi-app-flask
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline ejecutado correctamente'
        }
        failure {
            echo '❌ Algo falló en el pipeline'
        }
    }
}