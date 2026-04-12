pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'docker-creds'
        DOCKER_IMAGE = 'your-dockerhub-username/java-devops-app'
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/your-username/java-devops-project.git'
            }
        }

        stage('Build') {
            steps {
                dir('app') {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push $DOCKER_IMAGE:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }
    }
}