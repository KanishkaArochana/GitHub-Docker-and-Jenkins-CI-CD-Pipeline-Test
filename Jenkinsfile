// Jenkins Pipeline definition
pipeline {
    agent any // Run the pipeline on any available agent
    
    stages { 
        // Stage 1: Source Code Management (SCM) Checkout
        stage('SCM Checkout') {
            steps {
                // Retry the git checkout up to 3 times in case of transient failures
                retry(3) {
                    git branch: 'main', url: 'git@github.com:KanishkaArochana/GitHub-Docker-and-Jenkins-CI-CD-Pipeline-Test.git'
                }
            }
        }
        // Stage 2: Build Docker Image
        stage('Build Docker Image') {
            steps {  
                // Build the Docker image and tag it with the Jenkins build number
                bat 'docker build -t thimira123/nodeapp-cuban:%BUILD_NUMBER% .'
            }
        }
        // Stage 3: Login to Docker Hub
        stage('Login to Docker Hub') {
            steps {
                // Use Jenkins credentials to securely pass the Docker Hub password
                withCredentials([string(credentialsId: 'dockerpassword', variable: 'dockerpassword')]) {
                    script {
                        // Login to Docker Hub using the provided credentials
                        bat "docker login -u thimira123 -p ${dockerpassword}"
                    }
                }

            }
        }
        // Stage 4: Push Image
        stage('Push Image') {
            steps {
                retry(3) {
                    bat 'docker push thimira123/nodeapp-cuban:%BUILD_NUMBER%'
                }
            }
        }
    }
    post {
        // Always run this step after the pipeline (success or failure)
        always {
            // Logout from Docker Hub to clean up credentials
            bat 'docker logout'
        }
    }
}