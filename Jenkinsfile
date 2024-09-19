pipeline {
  agent {
    kubernetes {
      yamlFile 'demo.yaml'
      idleMinutes 1
    }
  }
  stages {
    stage('Build maven code') {
      steps {
      container('mavencontainer') {  
          sh "mvn clean install"
       }    
      }
    }
    stage('Build docker image') {
      steps {
        container('dockercontainer') {
          withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pass', usernameVariable: 'user')]) {
          sh '''
          docker login -u ${user} -p ${pass}
          docker build -t jsarvabhowma/demoapp:v1 .
          docker push jsarvabhowma/demoapp:v1
          '''
          }
        }
      }
    }
  }
}
