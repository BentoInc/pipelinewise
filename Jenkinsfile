pipeline {
  agent {
    label 'java11'
  }

  options {
    buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '5'))
  }

  parameters {
    string(name: 'image_tag', defaultValue: 'bento-pipelinewise', description: 'Output name of Docker image')
    string(name: 'connectors', defaultValue: 'tap-adwords,tap-google-analytics,tap-mysql,tap-postgres,tap-salesforce,tap-zendesk,tap-s3-csv,target-postgres', description: 'Comma separated list of connectors to build (use all to build all)')
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
