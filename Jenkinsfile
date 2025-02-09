pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "dockerrk11/myapp:latest" 
        KUBECONFIG = credentials('kubeconfig')    
    }

    stages {
        stage('Clone Repository') {
            steps {
<<<<<<< HEAD
               git branch: 'master', credentialsId: 'github-credentials', url: 'https://github.com/ritikkorde/cicd-project.git'
=======
                git branch: 'main', credentialsId: 'github-credentials', url: 'https://github.com/ritikkorde/cicd-project.git'
>>>>>>> 60bac28 (new)
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }

        stage('Push Docker Image to DockerHub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-login', url: '']) {
                    sh "docker push $DOCKER_IMAGE"
                }
            }
        }

        stage('Terraform Init & Apply') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-credentials'
                ]]) {
                    sh "terraform init"
                    sh "terraform apply -auto-approve"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                    sh "kubectl apply -f k8s/deployment.yaml"
                    sh "kubectl apply -f k8s/service.yaml"
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs!"
        }
    }
}

