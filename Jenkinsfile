pipeline {
  agent any
   parameters {
        choice(
            choices: ['major' , 'minor', 'patch'],
            description: 'How to bump?',
            name: 'REQUESTED_ACTION')
    }
  stages {
    stage('Build docker image') {
      steps {
        sh 'docker build . -f Dockerfile_bump_version -t cd_demo'
      }
    }
    stage('bump major') {
      when {
          expression { 
            params.REQUESTED_ACTION == 'major' 
          }
      }
      steps {
       echo "bump major" 
      }
    }
  }
}
