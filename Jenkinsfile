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
    stage('Deploying ECR image to EKS') {
      steps {
        sh 'kubectl create -f deploy.yml'
        sh 'kubectl create -f service.yml'
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
    } */
    /*
    stage('Removing container if it already exists') {
      agent {label 'master'}
      steps{
        sh '''#!/bin/bash
                x=$( docker container inspect -f '{{.State.Status}}' scanContainer )
                echo $x
                if [ $x == "running" ]
                then
                        sudo docker stop scanContainer
                        sudo docker rm scanContainer
                elif [ $x == "exited" ]
                then
                        sudo docker rm scanContainer
                        
                else
                        echo "Container does not exist"
                fi
                
                '''
          
       
      }
    }
    
    stage('Deploying to test server') {
      agent {label 'master'}
      steps{
        script {
          dockerImage.run('-itd --name scanContainer -p 8087:80')
          
        }
      }
    }  */
    /*stage('Deploy to eks cluster') {
      steps{
        sh 'kubectl create -f deploy.yml'
        sh 'kubectl create -f service.yml'
      }
    }

  }
}
*/
