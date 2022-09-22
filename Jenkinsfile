@Library('axyyadigitallibs') _

pipeline {
    agent any
  
    stages {
      stage ('Git clone') {
        steps{
          git 'https://github.com/Machendra-org/AXYYA-Digital.git'
        }
      }

      stage ('Build-Image') {
        steps{
          sh 'docker build -t node-app .'
        }
      }

      stage ('Push image to ECR') {
        steps{
          sh ''' docker tag node-app 883195043912.dkr.ecr.us-west-2.amazonaws.com/nodejs-repository:node-app
          aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin 883195043912.dkr.ecr.us-west-2.amazonaws.com
          docker push 883195043912.dkr.ecr.us-west-2.amazonaws.com/nodejs-repository:node-app
          '''
        }
      }

      stage ('Deploying app in K8s cluster') {
        steps{
          sh ''' kubectl apply -f axyyadigital.yml
          kubectl get pods 
          kubectl apply -f ingress.yml
          '''
        }
      }
      
    }
  }
