pipeline {
  agent any
  stages {
    stage('clone down') {
        agent {
             label 'master-label'
        }
          steps {
            stash excludes: '.git', name: 'code'
          }
        }
    stage('Parallel execution') {
      parallel {
        stage('Hello') {
          steps {
            sh 'echo "hello world"'
          }
        }
        stage('Build App') {
          agent {
            docker {
              image 'gradle:6-jdk11'
            }
          }
          options {
            skipDefaultCheckout true
            }

          steps {
            unstash 'code'
            sh 'ci/build-app.sh'
            archiveArtifacts 'app/build/libs/'
            sh 'ls -la'
            deleteDir()
            sh 'ls -la'
          }
        }
        
         stage('test app') {
          agent {
            docker {
              image 'gradle:6-jdk11'
            }
          }
          options {
            skipDefaultCheckout true
            }

          steps {
            unstash 'code'
            sh 'ci/unit-test-app.sh'
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
