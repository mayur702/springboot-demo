pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'maven3'
    }

    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/mayur702/springboot-demo.git'
            }
        }
        
        stage('Compile') {
            steps {
                sh "mvn compile"
            }
        }
        
        stage('Test') {
            steps {
                sh "mvn test"
            }
        }
        
        stage('Trivy FS Scan') {
            steps {
                sh "trivy fs --format table -o fs.html ."
            }
        }
        
        stage('Build') {
            steps {
                sh "mvn package"
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh '''$SCANNER_HOME/bin/sonar-scanner \
                            -Dsonar.projectName=spring-boot \
                            -Dsonar.projectKey=spring-boot \
                            -Dsonar.java.binaries=target'''
                    }
                }
            }
        }
        stage('Docker build and tag') {
            steps {
                sh "docker build -t mayur702/springboot:latest ."
            }
        }
        stage('Trivy image Scan') {
            steps {
                sh "trivy image mayur702/springboot:latest --format table -o image.html"
            }
        }
        stage('Docker Push Image') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'dockerhub') {
                        sh "docker push mayur702/springboot:latest"
                  }
                }
            }
        }
        stage('Deploy to kubernets'){
            steps{
                script{
                    withKubeConfig(caCertificate: '', clusterName: 'EKS_CLOUD', contextName: '', credentialsId: 'k8s', namespace: 'default', restrictKubeConfigAccess: false, serverUrl: 'https://F849396A4EF11327CB1BFB0A12A1D55E.gr7.ap-south-1.eks.amazonaws.com') {
                        sh "kubectl apply -f deployment.yaml"
                        sh "kubectl apply -f service.yaml"
                    }
                }
            }
        }
    }
}