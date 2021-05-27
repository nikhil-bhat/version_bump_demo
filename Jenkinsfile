pipeline {
  agent any
  stages {
    stage('Build docker image') {
      steps {
        sh 'docker build . -f Dockerfile_bump_version -t cd_demo'
        sh 'git config user.name "nikhil-bhat"'
        sh 'git config user.email nikhilbhat2008@gmail.com'
        echo 'Current version:'
        sh 'grep "Version: " README.md'
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
        withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                sh("git add README.md")
                sh("git commit -m "bump version")
                sh('git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/nikhil-bhat/version_bump_demo.git')
       }    
      }
    }

  }
  parameters {
    choice(choices: ['major' , 'minor', 'patch'], description: 'How to bump?', name: 'REQUESTED_ACTION')
  }
}
