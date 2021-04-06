pipeline {
  agent {
    docker {
      image 'gradle:jdk11'
    }

  }
  stages {
    stage('clone down') {
      agent {
        label 'swarm'
        }
      stash excludes: '.git', name: 'code'
    }

    stage('say hello') {
      parallel {
        stage('say hello') {
          steps {
            sh 'echo "hello world"'
          }
        }

        stage('build app') {
          steps {
            sh 'ci/build-app.sh'
            archiveArtifacts 'app/build/libs/'
            deleteDir()
            skipDefaultCheckout(true)
          }
        }

      }
    }

  }
}