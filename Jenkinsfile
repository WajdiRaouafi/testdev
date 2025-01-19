pipeline {
    agent any
    
    environment {
        // Set environment variables here if needed
        DOCKER_IMAGE = "project-devops:latest"
        // For pushing to Docker Hub, you might do:
        // DOCKER_HUB_REPO = "<your_dockerhub_username>/my-nest-app"
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Pull the code from your repository
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Check Node.js Version') {
            steps {
                sh 'node -v'
            }
        }
        stage('Install Node.js') {
            steps {
                sh '''
                    curl -sL https://deb.nodesource.com/setup_14.x | bash -
                    sudo apt-get install -y nodejs
                '''
            }
        }
        stage('Check Node.js Version in Docker') {
            steps {
                script {
                    sh "docker run --rm ${DOCKER_IMAGE} node -v"
                }
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        
        stage('Test') {
            steps {
                sh 'npm run test'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -f Dockerfile -t ${DOCKER_IMAGE} ."
                }
            }
        }
        
        // stage('Push Docker Image') {
        //     // when {
        //     //     expression {
        //     //         // Decide if you want to push from every build or only from certain branches
        //     //         return true
        //     //     }
        //     // }
        //     steps {
        //         script {
        //             // If pushing to Docker Hub:
        //             // withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', 
        //             //   usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        //             //   sh "docker login -u $USERNAME -p $PASSWORD"
        //             //   sh "docker tag ${DOCKER_IMAGE} ${DOCKER_HUB_REPO}:${env.BUILD_NUMBER}"
        //             //   sh "docker push ${DOCKER_HUB_REPO}:${env.BUILD_NUMBER}"
        //             // }

        //             // For local only, you can skip this.
        //             echo "Skipping push for local demonstration."
        //         }
        //     }
        // }
        
        // stage('Deploy to Minikube') {
        //     steps {
        //         script {
        //             // Option A: If you're pulling from Docker Hub or local registry:
        //             //   - Make sure minikube can pull your image
        //             //   - Possibly use `minikube docker-env` if needed
        //             //
        //             // Option B: Build inside minikube's Docker engine:
        //             //   - `eval $(minikube docker-env)`
        //             //   - build the image again
        //             //   - Then use your K8s manifests to deploy
        //             echo "Deploying to Minikube..."
                    
        //             // For simplicity, let's assume you push the image to Docker Hub or
        //             // you built it with minikube's Docker daemon. Then apply a K8s manifest:
        //             sh "kubectl apply -f k8s/deployment.yaml"
        //             sh "kubectl apply -f k8s/service.yaml"
        //         }
        //     }
        // }
    }
}
