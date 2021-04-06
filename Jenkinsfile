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
      steps {
        stash excludes: '.git', name: 'code'
      }
    }

    stage('say hello') {
      parallel {
        stage('say hello') {
          steps {
            sh 'echo "hello world"'
          }
        }

        stage('build app') {
          options {
            skipDefaultCheckout true
            }
          steps {
            sh 'ci/build-app.sh'
            archiveArtifacts 'app/build/libs/'
            unstash 'code'
          }
        }

        stage('test app') {
          steps {
            sh 'ci/unit-test-app.sh'
            junit 'app/build/test-results/test/TEST-*.xml'
            unstash 'code'
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