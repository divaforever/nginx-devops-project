pipeline {
    agent any

    environment {
        APP_NAME = "my-nginx-app"
        CONTAINER_NAME = "mywebsite"
        NGINX_SERVER = "18.144.234.108"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/divaforever/nginx-devops-project.git'
            }
        }

        stage('Copy Files to Nginx Server') {
            steps {
                sh '''
                scp -o StrictHostKeyChecking=no Dockerfile index.html ubuntu@$NGINX_SERVER:/home/ubuntu/nginx-devops-project/
                '''
            }
        }

        stage('Build Docker Image on Nginx Server') {
            steps {
                sh '''
                ssh -o StrictHostKeyChecking=no ubuntu@$NGINX_SERVER "
                    cd /home/ubuntu/nginx-devops-project && 
                    sudo docker build -t $APP_NAME .
                "
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                ssh -o StrictHostKeyChecking=no ubuntu@$NGINX_SERVER "
                    sudo docker stop $CONTAINER_NAME || true &&
                    sudo docker rm $CONTAINER_NAME || true
                "
                '''
            }
        }

        stage('Run New Container') {
            steps {
                sh '''
                ssh -o StrictHostKeyChecking=no ubuntu@$NGINX_SERVER "
                    sudo docker run -d --name $CONTAINER_NAME -p 8081:80 $APP_NAME
                "
                '''
            }
        }

    }
}
