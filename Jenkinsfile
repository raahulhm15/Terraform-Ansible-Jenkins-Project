@Library('github.com/releaseworks/jenkinslib') _

pipeline {
    agent any
    environment {
        registry = "334982178958.dkr.ecr.us-east-1.amazonaws.com/upgradproject/latest"
    }

    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'raahulhm', url: 'https://github.com/raahulhm15/Terraform-Ansible-Jenkins-Project.git']]])
            }
        }

    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry
        }
      }
    }

    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
        steps{
            script {
                sh 'docker login -u AWS -p $(aws ecr get-login-password --region us-east-1) 334982178958.dkr.ecr.us-east-1.amazonaws.com/upgradproject/latest '
                
                sh 'docker push 334982178958.dkr.ecr.us-east-1.amazonaws.com/upgradproject/latest:latest'
            }
        }
    }

    stage('Docker Run') {
     steps{
         script {
             sshagent(credentials : ['aws_ec2']){

                sh 'ssh -o StrictHostKeyChecking=no -i assignment-c7key.pem ubuntu@10.0.2.84'

             }
                //sh 'ssh -i /login/-i assignment-c7key.pem ubuntu@10.0.2.84'
                sh 'docker run -d -p 8081:8080  node 334982178958.dkr.ecr.us-east-1.amazonaws.com/upgradproject/latest'
            }
      }
    }
    }
}
