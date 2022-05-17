pipeline {
    agent any
    environment {
        registry = "638318277465.dkr.ecr.us-east-1.amazonaws.com/c7-assignment/node:alpine"
    }

    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/sirishavsp/finalProject.git']]])
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
                sh ' docker login -u AWS -p $(aws ecr get-login-password --region us-east-1) 150172465894.dkr.ecr.us-east-1.amazonaws.com/node'
                sh ' docker push 150172465894.dkr.ecr.us-east-1.amazonaws.com/node'
         }
        }
      }

    stage('Docker Run') {
     steps{
         script {
                sh ' ssh -i /home/ubuntu/finalProject.pem ubuntu@10.0.2.11'
                sh ' docker run -d -p 8080:8080 --rm --name node 150172465894.dkr.ecr.us-east-1.amazonaws.com/node:alpine'
            }
      }
    }
    }
}
