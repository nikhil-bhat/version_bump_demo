pipeline {
  agent any
  stages {
    stage('Build docker image') {
      steps {
        sh 'docker build . -f Dockerfile_bump_version -t cd_demo'
      }
    }
  }
}
