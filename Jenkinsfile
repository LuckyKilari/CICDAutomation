pipeline {
    agent any
    environment { 	
	IMAGE_NAME = 'luckykilari/sales-dashboard' 
	IMAGE_TAG = 'latest'
	}
    // parameters {
    //     string(name: 'REPO_NAME', defaultValue: 'luckykilari', description: 'repository name')        
    // }	

    stages {
        stage('Checkout') {
            steps {
		git branch: 'Dev_Test', url: 'https://github.com/LuckyKilari/CICDAutomation.git'   
	                          
            }
        }
	stage('Docker Login') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'dockerhub', 
                        usernameVariable: 'DOCKER_USERNAME', 
                        passwordVariable: 'DOCKER_PASSWORD'
                    )]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    }
                }
            }
        }    

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    dockerImage.push()
                }
            }
        }
        }
    }

