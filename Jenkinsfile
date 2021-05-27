pipeline {
  agent any
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
        echo 'bump major'
        sh 'docker run -v `pwd`:/workspace -w /workspace cd_demo bumpversion major'
      }
    }

  }
  parameters {
    choice(choices: ['major' , 'minor', 'patch'], description: 'How to bump?', name: 'REQUESTED_ACTION')
  }
}