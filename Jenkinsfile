pipeline {
  agent {
    label 'java11'
  }

  options {
    buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '5'))
  }

  parameters {
    string(name: 'image_tag', defaultValue: 'bento-pipelinewise', description: 'Output name of Docker image')
    string(name: 'connectors', defaultValue: 'all', description: 'Connectors to enable')
  }

  stages {
    stage('build') {
      steps {
          sh '''
          docker build --build-arg connectors="${connectors}" -t "${image_tag}" .
          '''
      }
    }
  }
}
