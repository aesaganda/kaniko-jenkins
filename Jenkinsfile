pipeline {

  agent {
    kubernetes {
      label 'docker-agent'
    }
  }
  
  environment {
    DOCKER_CONFIG = "/kaniko/.docker"  // 🟢 Required for Kaniko to find credentials
  }
  
  stages {
    stage('Kaniko Build & Push Image') {
      steps {
        container('kaniko') {
          script {
            sh '''
              echo ">> Debug Docker config"
              ls -la $DOCKER_CONFIG
              cat $DOCKER_CONFIG/config.json || echo "config.json not found"

              echo ">> Starting Kaniko build"
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
