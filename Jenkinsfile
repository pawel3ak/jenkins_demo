pipeline {
  agent any
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
      }
    }
    stage('Deploy') {
      steps {
        sh 'mvn package'
      }
    }
    stage('Publish') {
        steps {
            script {
                def server = Artifactory.server 'jfrog'
                def uploadSpec = """{
                    "files": [
                        {
                            "pattern": "terget/jenkinsDemo*",
                            "target": "my-pipeline-results"
                        }
                    ]
                }"""
                server.upload(uploadSpec)
            }
        }
    }
  }
  environment {
    proxy = 'http://localhost:proxyPort'
  }
}