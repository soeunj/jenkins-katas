pipeline {
  agent any
  stages {
    stage('Say hello') {
      parallel {
        stage('say hello') {
          steps {
            sh 'echo "hello world"'
          }
        }

        stage('build app') {
          steps {
            sh 'ci/build-app.sh'
          }
        }

      }
    }

  }
}