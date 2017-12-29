pipeline {
    agent any
    tools {
        maven 'localMaven'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean call-me-package'
            }
            post {
              success {
                  echo 'Now Archiving'
                  archiveArtifacts artifacts: '**/*.war'
              }
            }
        }

    }
}