pipeline {
  agent {
    kubernetes {
      label 'docker-agent'
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    command:
    - cat
    tty: true
    volumeMounts:
    - name: docker-config
      mountPath: /kaniko/.docker
  volumes:
  - name: docker-config
    secret:
      secretName: regcred
      items:
      - key: .dockerconfigjson
        path: config.json
"""
    }
  }

  stages {
    stage('Debug Docker Config') {
      steps {
        container('kaniko') {
          sh '''
            echo "Listing /kaniko/.docker directory:"
            ls -la /kaniko/.docker
            echo "Contents of config.json:"
            cat /kaniko/.docker/config.json || echo "config.json not found"
          '''
        }
      }
    }

    // stage('Kaniko Build & Push Image') {
    //   steps {
    //     container('kaniko') {
    //       script {
    //         sh '''
    //           /kaniko/executor \
    //             --dockerfile `pwd`/Dockerfile \
    //             --context `pwd` \
    //             --destination=aesaganda/kaniko-test:${BUILD_NUMBER} \
    //             --cache=true
    //         '''
    //       }
    //     }
    //   }
    // }
  }
}
