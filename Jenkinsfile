pipeline {
  agent any
  stages {
    stage('Build docker image') {
      steps {
        sh 'docker build . -f Dockerfile_bump_version -t cd_demo'
        sh 'git config user.name "nikhil-bhat"'
        sh 'git config user.email nikhilbhat2008@gmail.com'
        echo 'Current version:'
        sh 'cat README.md'
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
        sh 'cat README.md'
        withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                sh("git add README.md")
                sh("git commit -m 'bump version' --allow-empty ")
                sh('git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/nikhil-bhat/version_bump_demo.git')
       }    
      }
    }
    
    
    
      stage('bump minor') {
      when {
        expression {
          params.REQUESTED_ACTION == 'minor'
        }

      }
      steps {
        echo 'bump minor'
        sh 'docker run -v `pwd`:/workspace -w /workspace cd_demo bumpversion minor'
        withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                sh('git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/nikhil-bhat/version_bump_demo.git')
       }    
      }
        
        
          stage('bump patch') {
      when {
        expression {
          params.REQUESTED_ACTION == 'patch'
        }

      }
      steps {
        echo 'bump patvh'
        sh 'docker run -v `pwd`:/workspace -w /workspace cd_demo bumpversion patvh'
        withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                sh('git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/nikhil-bhat/version_bump_demo.git')
       }    
      }

  }
  parameters {
    choice(choices: ['major' , 'minor', 'patch'], description: 'How to bump?', name: 'REQUESTED_ACTION')
  }
  post {
    always {
        echo 'Cleaning workspace'
        deleteDir()
    }
  }
}
