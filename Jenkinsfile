pipeline {
    agent any
    environment { 	
	IMAGE_NAME = 'luckykilari/sales-dashboard' 
	IMAGE_TAG = 'latest'
	}
    parameters {
        string(name: 'REPO_NAME', defaultValue: 'luckykilari', description: 'repository name')        
    }	

    stages {
        stage('Checkout') {
            steps {
		git branch: 'Dev_Test', url: 'https://github.com/LuckyKilari/CICDAutomation.git'   
	                          
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${params.REPO_NAME} ."
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

