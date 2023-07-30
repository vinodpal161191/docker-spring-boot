pipeline {

    
    agent any

    environment {
        registry = "554723871506.dkr.ecr.ap-south-1.amazonaws.com/springbootjenkins"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/vinodpal161191/docker-spring-boot']])
            }
        }
        
        stage ("Build JAR") {
            steps {
                sh "mvn clean install"
            }
        }
        
        stage ("Build Image") {
            steps {
                script {
                    docker.build registry
                }
            }
        }
        
        stage ("Push to ECR") {
            steps {
                script {
                    sh "aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 554723871506.dkr.ecr.ap-south-1.amazonaws.com"
                    sh "docker push 554723871506.dkr.ecr.ap-south-1.amazonaws.com:latest"
                    
                }
            }
        }
        
        stage ("Helm package") {
            steps {
                    sh "helm package springboot"
                }
            }
                
        stage ("Helm install") {
            steps {
                    sh "helm upgrade myrelease-v1 springboot-0.1.0.tgz"
                }
            }
    }
}
