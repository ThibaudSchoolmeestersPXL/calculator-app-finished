pipeline {
    agent any
    
    tools {
        nodejs "nodetin"
    }
    
    stages {
        stage('Print versions and debug info') {
            steps {
                sh 'whoami'
                sh 'node -v'
                sh 'npm -v'
            }
        }
        stage('opdracht 5') {
            steps {
                sh 'pwd'
                sh 'rm -rf ./*'
            }
        }
        stage('checkout code') {
            steps {
                git branch: 'main', url: 'https://github.com/ThibaudSchoolmeestersPXL/calculator-app-finished.git'
            }
        }

        stage('install dependencies') {
          steps {
            sh 'npm install'
          }
        }

        stage('unittest') {
          steps {
            sh 'npm run test'
            junit "junit.xml"
            sh 'ls -lah'
          }
        }

        stage('create bundle') {
          steps {
            sh 'mkdir bundle'
            sh 'cp -r node_modules bundle'
            sh 'cp -r public bundle'
            sh 'cp *.js* bundle'
            sh 'cp Dockerfile bundle'
            sh 'cp docker-compose.yml bundle'
            sh 'ls -lah ./bundle'
            sh 'zip bundle.zip bundle'
          }
        }
    }

    post {
      success {
        sh 'mkdir artifacts'
        archiveArtifacts artifacts: 'bundle.zip', fingerprint: true
      }

      failure {
        script {
          def errorLogPath = '/var/lib/jenkins/jenkinserrorlog'
          def currentDate = new Date().toString()
          def errorMessage = "pipeline poging faalt op $currentDate"

          writeFile file: errorLogPath, text: errorMessage
        }
      }
    }
}
