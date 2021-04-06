pipeline {
  agent any
  stages {
    stage("Clone Down") {
      agent { node "master-label" }
      steps {
          stash excludes: '.git', name: 'code'
      }
    }
    stage('Say Hello') {
      parallel {
        stage('Say Hello') {
          steps {
            sh 'echo "Hello world!"'
          }
        }

        stage('Build app') {
          options {
            skipDefaultCheckout(true)
          }
          agent {
            docker {
              image 'gradle:jdk11'
            }

          }
          steps {
            unstash "code"
            sh 'ci/build-app.sh'
            archiveArtifacts 'app/build/libs/'
          }
        }

      }
    }

  }
}