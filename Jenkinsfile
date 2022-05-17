pipeline {
    agent any
    environment {
        registry = "638318277465.dkr.ecr.us-east-1.amazonaws.com/c7-assignment/node:alpine"
    }

    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/sumeetkhastgir/C7-Assignment.git']]])
            }
        }

    // Building Docker images
    stage('Building image') {
      steps{
        script {
          sh  'docker build -t nodeimage .'
        }
      }
    }

    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{
         script {
                sh ' docker login -u AWS -p $(aws ecr get-login-password --region us-east-1) 638318277465.dkr.ecr.us-east-1.amazonaws.com/c7-assignment/nodeimage:latest'
                sh ' docker tag c7-assignment:latest 638318277465.dkr.ecr.us-east-1.amazonaws.com/c7-assignment:latest'
                sh ' docker push 638318277465.dkr.ecr.us-east-1.amazonaws.com/c7-assignment:latest'
         }
        }
      }

    stage('Docker Run') {
     steps{
         script {
                sh ' ssh -i /home/ubuntu/assignment-c7key.pem ubuntu@54.167.100.63'
                sh ' docker run -d -p 8080:8080 --rm --name node 638318277465.dkr.ecr.us-east-1.amazonaws.com/c7-assignment/node:alpine'
            }
      }
    }
    }
}
