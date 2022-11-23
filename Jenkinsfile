pipeline {
  agent any
  stages {
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
          steps {
            sh 'ci/build-app.sh'
            archiveArtifacts 'app/build/libs/'
            sh 'ls -la'
            deleteDir()
            sh 'ls -la'
          }
        }

      }
    }

  }
}
