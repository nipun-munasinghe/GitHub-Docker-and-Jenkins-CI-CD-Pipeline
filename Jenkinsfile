pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/nipun-munasinghe/GitHub-Docker-and-Jenkins-CI-CD-Pipeline'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t nipunl/nodeapp-cuban:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'tset-dockerhubpassword', variable: 'test-docker-pass')]) {
                    bat 'docker login -u nipunl -p ${test-docker-pass}'
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push nipunl/nodeapp-cuban:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}