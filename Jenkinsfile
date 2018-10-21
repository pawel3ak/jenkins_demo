pipeline {
  agent none
  stages {
    stage('Build') {
      steps {
        dir(path: 'maven-example/')
        sh 'mvn clean install package'
      }
    }
    stage('Test') {
      parallel {
        stage('Test') {
          steps {
            echo 'Maven testing only now'
            timeout(time: 180) {
              junit(allowEmptyResults: true, testResults: 'target/testResults.xml')
            }

          }
        }
        stage('API') {
          steps {
            echo 'Platform 1-A step'
          }
        }
        stage('Smoke') {
          steps {
            echo 'Platform 2-A test step'
            sh 'mvn clean verify -Dtags=\'type:Smoke\''
            catchError() {
              sh 'mvn clean verify -Dtags=\'type:Smoke\''
            }

          }
        }
      }
    }
    stage('Deploy') {
      steps {
        sh 'mvn package'
      }
    }
  }
  environment {
    proxy = 'http://localhost:proxyPort'
  }
}