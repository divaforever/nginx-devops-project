pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/divaforever/nginx-devops-project.git'
            }
        }

        stage('Deploy to Nginx Server') {
            steps {
                sh '''
                scp -o StrictHostKeyChecking=no index.html ubuntu@52.53.125.85:/tmp/

                ssh -o StrictHostKeyChecking=no ubuntu@52.53.24.160 "
                    sudo cp /tmp/index.html /var/www/html/index.html
                "
                '''
            }
        }

    }
}
