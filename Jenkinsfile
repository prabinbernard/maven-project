pipeline {
    agent any

    tools {
        maven 'localMaven'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '10.0.15.40', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '10.0.15.50', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
              success {
                  echo 'Now Archiving'
                  archiveArtifacts artifacts: '**/*.war'
              }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp **/target/*.war root@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp **/target/*.war root@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }



    }
}
