pipeline {
  agent any
   environment {
      dockerhub=credentials('dockerhub')
   }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5', daysToKeepStr: '5'))
    timestamps() 
    timeout(time: 20, unit: 'MINUTES') 
  }
  
  stages {  
    stage('Checkout') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/vaibhavkumar779/FinalDevOpsKUP.git']]])
      }
    }
    stage('Setup') { 
      steps {
          sh " pip3 install -r requirements.txt "
      }
    }
    
    stage('Linting') { 
      steps {
        script {
          sh """
          pylint -E **/*.py
          """
        }
      }
    }
 
    

   
     stage("building docker image"){
                    steps{
                      sh 'docker build -t capstone:${GIT_COMMIT} .'
                       
                     }    
                }
            stage("Pushing the docker image"){
                    steps{
                      sh 'echo $dockerhub_PSW | docker login -u $dockerhub_USR --password-stdin'
                      sh 'docker tag capstone:${GIT_COMMIT} vaibhavkuma779/flaskDockerK:${GIT_COMMIT}'
                      sh 'docker push  vaibhavkuma779/flaskDockerK:${GIT_COMMIT}'
                      sh 'docker tag capstone:${GIT_COMMIT} vaibhavkuma779/flaskDockerK:latest'
                      sh 'docker push  vaibhavkuma779/flaskDockerK:latest'
                    }
                }

  }
} 
