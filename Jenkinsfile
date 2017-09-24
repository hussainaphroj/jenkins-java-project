pipeline {
  agent none

  environment {
    MAJOR_VERSION = 1
  }

  stages {
    stage('Say Hello') {
      agent any

      steps {
        sayHello 'Awesome Student!'
      }
    }
    stage('Git Information') {
      agent any

      steps {
        echo "My Branch Name: ${env.BRANCH_NAME}"

        script {
          def myLib = new linuxacademy.git.gitStuff();

          echo "My Commit: ${myLib.gitCommit("${env.WORKSPACE}/.git")}"
        }
      }
    }
    stage('Unit Tests') {
      agent {
        label 'apache'
      }
      steps {
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }
    stage('build') {
      agent {
        label 'apache'
      }
      steps {
        sh 'ant -f build.xml -v'
      }
      post {
        success {
          archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
        }
      }
    }
  }
}
