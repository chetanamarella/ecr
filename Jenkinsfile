pipeline {
  agent any
  environment {
    registry = "734468820065.dkr.ecr.us-east-2.amazonaws.com/ecr-repo"
    dockerImage = ''
    
  }
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/chetanamarella/ecr.git'
      }
    }
    stage('Building image') {
      steps {
        script {
          dockerImage = docker.build registry + ":latest"
        }
      }
    }
    stage('Pushing Image to AWS ECR') {
      steps {
        script {
          sh 'aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 734468820065.dkr.ecr.us-east-2.amazonaws.com'
          sh 'docker push 734468820065.dkr.ecr.us-east-2.amazonaws.com/ecr-repo:latest'
          }
        }
      }
    stage('Remove files if they already exist') {
      steps{
        sh '''#!/bin/bash
                file1=/var/lib/jenkins/workspace/deploy-eks/deploy.yml
                if [ -f "$file1" ]; then
                  sh 'kubectl delete deployment eks-deploy'
                fi
                
                file2=/var/lib/jenkins/workspace/deploy-eks/service.yml
                if [ -f "$file2" ]; then
                  sh 'kubectl delete svc eks-service'
                fi
          
              
                '''
      }
    } 
    stage('Deploying ECR image to EKS') {
      steps {
        withAWS(credentials: 'aws', region: 'us-east-2') {
        sh 'kubectl create -f deploy.yml'
        sh 'kubectl create -f service.yml'
        }
      }
    }
  }
}

 

   /* stage('Scan Image') {
      agent {label 'master'}
      steps{
        script {
          def imageLine = 'chetana3/scan:latest'
          writeFile file: 'anchore_images', text: imageLine
          anchore name: 'anchore_images' 
        }
      }
    }
    
   
    
    stage('Deploying to test server') {
      agent {label 'master'}
      steps{
        script {
          dockerImage.run('-itd --name scanContainer -p 8087:80')
          
        }
      } */
