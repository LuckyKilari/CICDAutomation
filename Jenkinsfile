pipeline {
    agent any
    environment { 
	BRANCH_NAME = 'Dev_Test'
	IMAGE_NAME = 'luckykilari/sales-dashboard' 
	IMAGE_TAG = 'latest'
	}    

    stages {
        stage('Checkout Specific Branch') {
            steps {
	        git branch: "${BRANCH_NAME}",
                    url: 'https://github.com/LuckyKilari/CICDAutomation.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Docker Login & Push') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                        sh "docker push ${IMAGE_NAME}"
                    }
                }
            }
        }
    }
}

