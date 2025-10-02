pipeline {
    agent any

    environment {
        REPO = "rex5656/ci-cd-demo"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ramkumar-24/ci-cd-demo-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t $REPO:latest .'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-pass',
                    usernameVariable: 'DOCKERHUB_USER',
                    passwordVariable: 'DOCKERHUB_PASS'
                )]) {
                    sh 'echo $DOCKERHUB_PASS | docker login -u $DOCKERHUB_USER --password-stdin'
                    sh 'docker push $REPO:latest'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                  docker rm -f ci-cd-demo || true
                  docker run -d --name ci-cd-demo -p 3000:3000 $REPO:latest
                '''
            }
        }
    }
}
