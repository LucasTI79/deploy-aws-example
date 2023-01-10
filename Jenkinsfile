pipeline {
  agent any

  stages {
   stage('Build project image') {
    steps {
      script {
        dockerapp = docker.build("danieleleaoe/deploy-aws:${env.BUILD_ID}", "-f ./Dockerfile ./")
      }
    }
   }

   stage ('Remove container if exists') {
      steps  {
          sh '''
            if [ $( docker ps -a -f name=deploy-aws | wc -l ) -eq 2 ]; then
              docker stop deploy-aws
              docker rm deploy-aws
            
            fi
          '''
   
      }
    }

   stage('Deploy image') {
    steps {
      sh """
        docker run --name deploy-aws -p 3000:3000 -d -t danieleleaoe/deploy-aws:${env.BUILD_ID}
      """
    }
   }
  }
}