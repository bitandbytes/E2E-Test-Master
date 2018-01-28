pipeline {
  agent {
    node {
      label 'master'
    }
    
  }
  stages {
    stage('Smoke Deployment') {
      agent any
      steps {
        echo 'Smoke Deployment'
      }
    }
    stage('Smoke Test') {
      agent any
      steps {
        echo 'Smoke Test'
      }
    }
    stage('E2E Master') {
      agent any
      steps {
        echo 'E2E Master'
      }
    }
    stage('Optimums Plan') {
      agent any
      steps {
        echo 'Optimums Plan Started'
      }
    }
    stage('E2E Test') {
      agent {
        node {
          label 'master'
        }
        
      }
      steps {
        bat(returnStdout: true, script: 'TEST_JAR_EXECUTER')
      }
    }
  }
}