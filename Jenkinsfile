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
                    sudo apt-get install -y python3-pip
                    pip3 install -r app/requirements.txt
                    python3 -m pytest tests/
                '''
            }
        }

        stage('Build con Docker') {
            steps {
                sh 'sudo docker build -t mi-app-flask ./app'
            }
        }

        stage('Deploy local con Docker') {
            steps {
                sh '''
                    sudo docker stop mi-app-flask || true
                    sudo docker rm mi-app-flask || true
                    sudo docker run -d --name mi-app-flask -p 5000:5000 mi-app-flask
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