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

        stage('Test app') {
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
            sh "ci/unit-test-app.sh"
            junit 'app/build/test-results/test/TEST-*.xml'
          }
        }

      }
    }

  }
  post {
    cleanup {
        deleteDir() /* clean up our workspace */
    }
  }
}