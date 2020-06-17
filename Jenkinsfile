pipeline {
  agent {
    label 'java11'
  }

  options {
    buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '5'))
  }

  parameters {
    booleanParam(name: 'deploy', defaultValue: false, description: 'Deploy to ECR')
    string(name: 'connectors', defaultValue: 'tap-adwords,tap-google-analytics,tap-mysql,tap-postgres,tap-salesforce,tap-zendesk,tap-s3-csv,target-postgres', description: 'Comma separated list of connectors to build (use all to build all)')
  }

  stages {
    stage('build') {
      steps {
          sh '''
          aws_account=$(aws sts get-caller-identity --output text | cut -f1)
          docker build --build-arg CONNECTORS="${connectors}" -t "${aws_account}.dkr.ecr.us-west-2.amazonaws.com/pipelinewise" .
          '''
      }
    }

    stage('deploy') {
      when {
        expression {params.deploy == true}
      }
      steps {
        sh '''
          $(aws ecr get-login --no-include-email --region us-west-2)
          aws_account=$(aws sts get-caller-identity --output text | cut -f1)
          docker push "${aws_account}.dkr.ecr.us-west-2.amazonaws.com/pipelinewise"
        '''
      }
    }
  }
}
