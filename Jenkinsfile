pipeline {
  agent any
  stages {
    stage('install tools') {
      steps {
        withPythonEnv('python3') {
    sh 'python --version'
       }
      }
    }

  }
}
