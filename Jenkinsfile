pipeline {
  agent {
    node {
      label 'master'
    }
    
  }
  stages {
    stage('Smoke Deployment') {
      agent {
        node {
          label 'master'
        }
        
      }
      steps {
        echo 'Smoke Deployment'
      }
    }
    stage('Smoke Test') {
      agent {
        node {
          label 'master'
        }
        
      }
      steps {
        echo 'Smoke Test'
      }
    }
    stage('E2E Master') {
      agent {
        node {
          label 'master'
        }
        
      }
      steps {
        echo 'E2E Master'
      }
    }
    stage('Optimums Plan') {
      agent {
        node {
          label 'master'
        }
        
      }
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
        bat 'java -jar -Dfilters=-skip -Dstory=%THREAD_1_STORIES% ..\\endtoend-tests-1.0.0.0-SNAPSHOT-ccp-e2etest.jar'
      }
    }
  }
  environment {
    THREAD_1_STORIES = 'MyStory'
  }
}