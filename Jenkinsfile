pipeline {

  agent {
    kubernetes {
      label docker-agent
    }
  }

  stages {

    stage('Kaniko Build & Push Image') {
      steps {
        container('kaniko') {
          script {
            sh '''
            /kaniko/executor --dockerfile `pwd`/Dockerfile \
                             --context `pwd` \
                             --destination=aesaganda/kaniko-test:${BUILD_NUMBER}
            '''
          }
        }
      }
    }

    // stage('Deploy App to Kubernetes') {     
    //   steps {
    //     container('kubectl') {
    //       withCredentials([file(credentialsId: 'mykubeconfig', variable: 'KUBECONFIG')]) {
    //         sh 'sed -i "s/<TAG>/${BUILD_NUMBER}/" myweb.yaml'
    //         sh 'kubectl apply -f myweb.yaml'
    //       }
    //     }
    //   }
    // }
  
  }
}
