pipeline {
    agent any
    tools {
        maven 'M2_HOME'
    }
    environment {
        registry = '889884066605.dkr.ecr.ap-south-1.amazonaws.com/repolara'
        registryCredentials = 'jenkins-ecr-login-credentials'
        dockerImage = ''
    }
    stages {
        stage('checkout the project') {
            steps {
                git branch: 'main', url: 'https://github.com/cbabu85/java-maven-pipeline-lara.git'
            }
        }
        stage('Build the Package') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Deploy the Image to Amazon ECR') {
            steps {
                script {
                    docker.withRegistry( "http://" + registry, "ecr:ap-south-1:" + registryCredentials ) {
                    dockerImage.push()
                    }
                }
            }
        }
    }
}
