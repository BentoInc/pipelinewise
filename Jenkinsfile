pipeline {
  agent {
    label 'java11'
  }

  options {
    buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '5'))
  }

  parameters {
    string(name: 'image_tag', defaultValue: 'bento-pipelinewise', description: 'Output name of Docker image')
  }

  stages {
    stage('build') {
      steps {
          sh '''
          docker build --build-arg CONNECTORS="${connectors}" -t "${image_tag}" .
          '''
      }
    }
  }
}
