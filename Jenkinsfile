pipeline {
  agent any
  triggers {
         pollSCM('* * * * *')
  }

  stages {
    stage('Cloning Git') {
        steps {
             git branch: 'dev', credentialsId: 'ShopA', poll: false, url: 'https://github.com/vuongpham30/ShopA.git'
        }
    }

    stage('Buil and push image to registry') {
        steps {
             sh "docker buil -t harbor.vn/shopa/shopa-image:latest ."
             sh "docker tag harbor.vn/shopa/shopa-image:latest harbor.vn/shopa/shopa-image:${BUILD_NUMBER} "
             sh "docker push harbor.vn/shopa/shopa-image:${BUILD_NUMBER}"
             sh "docker rmi harbor.vn/shopa/shopa-image:${BUILD_NUMBER} "
        }
    }
    
    stage('SonarQubeScanner') {
    //We have to setup Sonar tool on jenkins before
      steps {
        script {
          scannerHome = tool 'SonarQubeScanner'
        }
      withSonarQubeEnv('sonar') {
          sh "${scannerHome}/bin/sonar-scanner "
      }
    } 
  }
}
}
