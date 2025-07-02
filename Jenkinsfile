pipeline {
    agent any
    environment { 
		DOCKER_IMAGE = 'luckykilari/sales-dashboard' 
		IMAGE_TAG = 'latest'
		DOCKER_CREDENTIALS_ID = 'dockerhub' 

		}     	

    stages {
        stage('Checkout') {
            steps {
			    git branch: 'Dev_Test', url: 'https://github.com/LuckyKilari/CICDAutomation.git'                
            }
        }
		stage('Build/Compile') {
            steps {
                echo "Running Python Linting and Build checks"
                sh 'pip install -r requirements.txt'
                sh 'python -m py_compile $(find . -name "*.py")'  // Compiles Python files                
            }
        }
		
		stage('Docker Build') {
            steps {
                script {
                    sh "docker build -t $DOCKER_IMAGE:${env.BUILD_NUMBER} ."
                }
            }
        }
 
        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('https://hub.docker.com/repositories/luckykilari', DOCKER_CREDENTIALS_ID) {
                        sh "docker tag $DOCKER_IMAGE:${env.BUILD_NUMBER} $DOCKER_IMAGE:latest"
                        sh "docker push $DOCKER_IMAGE:${env.BUILD_NUMBER}"
                        sh "docker push $DOCKER_IMAGE:latest"
                    }
                }
            }
        }      

        
    }
}

