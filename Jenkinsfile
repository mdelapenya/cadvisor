@Library('ReleaseManagementSharedLib') _

pipeline {
  agent {
    node {
      label 'lax'
    }
  }
  environment {
    GOPATH = "/mnt/go"
    PATH = "$GOPATH/bin:$PATH"
  }
  stages {
    stage('Build') {
      steps {
        buildStep('Build') {
          sh 'go get -d github.com/google/cadvisor'
          sh 'sudo mkdir -p /mnt/go/src/github.com/wedeploy && sudo ln -fs $WORKSPACE /mnt/go/src/github.com/google/cadvisor'
          sh 'cd /mnt/go/src/github.com/google/cadvisor && make build'
        }
      }
    }
    stage('Tests') {
      steps {
        buildStep('Unit Tests') {
          sh 'cd /mnt/go/src/github.com/google/cadvisor && make test'
        }
      }
      post {
        always {
          junit(allowEmptyResults: true, testResults: '**/test-results/**/TEST-*.xml')
        }
      }
    }
  }
}