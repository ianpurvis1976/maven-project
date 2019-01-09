pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '34.244.237.194', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '34.240.250.112', description: 'Production Server')
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
                        bat "pscp -i C:/Users/PURVISIS/Downloads/tomcat.pem target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "pscp -i C:/Users/PURVISIS/Downloads/tomcat.pem target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}