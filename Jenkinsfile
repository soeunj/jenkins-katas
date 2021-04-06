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
            unstash 'code'
            sh 'ci/build-app.sh'
            archiveArtifacts 'app/build/libs/'
            stash name: 'code'
          }
        }

        stage('test app') {
          steps {
            unstash 'code'
            sh 'ci/unit-test-app.sh'
            junit 'app/build/test-results/test/TEST-*.xml'
          }
        }
      }
    }

    stage('push docker app') {
      environment {
        DOCKERCREDS = credentials('docker_login') //use the credentials just created in this stage
      }
      steps {
        unstash 'code' //unstash the repository code
        sh 'ci/build-docker.sh'
        sh 'echo "$DOCKERCREDS_PSW" | docker login -u "$DOCKERCREDS_USR" --password-stdin' //login to docker hub with the credentials above
        sh 'ci/push-docker.sh'
      }
    }
  }
  post {
    cleanup {
        deleteDir() /* clean up our workspace */
    }
  }
}