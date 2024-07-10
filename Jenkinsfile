pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub'
        DOCKER_IMAGE = 'nodejs-random-color'
        IMAGE_TAG = "lastest"
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/haopham98/nodejs-random-color.git'
            }
        }

        stage('Build') {
            steps {
                def imageTag = "${DOCKER_IMAGE}:ver-${BUILD_ID}"
                env.IMAGE_TAG = imageTag
                sh "docker build -t ${imageTag} ."
            }
        }
        
        stage('Login to Docker Hub') {
            steps {
                script {
                    // Đăng nhập vào Docker Hub
                    withCredentials([usernamePassword(credentialsId: env.DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    }
                }
            }
        }
        stage('Upload image to Dockerhub') {
            steps {
                // sh 'aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin 430950558682.dkr.ecr.ap-southeast-1.amazonaws.com'
                // sh 'docker tag nodejs-random-color:ver-${BUILD_ID} 430950558682.dkr.ecr.ap-southeast-1.amazonaws.com/nodejs-random-color:ver-${BUILD_ID}'
                // sh 'docker push 430950558682.dkr.ecr.ap-southeast-1.amazonaws.com/nodejs-random-color:ver-${BUILD_ID}'
            	sh 'docker tag nodejs-random-color:ver-${BUILD_ID} hoanghao98/nodejs-random-color:ver-${BUILD_ID}'
				sh 'docker push hoanghao98/nodejs-random-color:ver-${BUILD_ID}'
			}
        }

        stage('Deploy image') {
            steps {
                sh "envsubst < docker-compose.yml | docker-compose -f - up -d"
            }
        }
    }
}
