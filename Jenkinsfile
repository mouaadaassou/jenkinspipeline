pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: 'http://localhost:9091', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'http://localhost:9093', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "cp -i /home/jenkins/tomcat-demo.pem **/target/*.war ${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
