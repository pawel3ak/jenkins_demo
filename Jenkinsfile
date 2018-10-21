#!groovy​

pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'mvn clean install'
      }
    }
    stage('Test') {
      parallel {
        stage('Test') {
          steps {
            timeout(time: 180) {
                echo 'Maven testing only now'
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
                            "pattern": "target/jenkinsDemo*",
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