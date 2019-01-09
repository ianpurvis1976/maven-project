pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '34.244.237.194', description: 'Staging Server')
         string(name: 'tomcat_dev_hostkey', defaultValue: 'ca:b0:c7:68:eb:e8:79:bf:ba:96:6f:22:e0:e4:e5:05', description: 'puttygen host key')
         string(name: 'tomcat_prod', defaultValue: '34.240.250.112', description: 'Production Server')
         string(name: 'tomcat_prod_hostkey', defaultValue: '  49:e4:3f:26:c5:22:dc:d5:fa:8a:42:aa:21:7f:cd:fe', description: 'puttygen host key')
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
                        bat "pscp -batch -hostkey ${params.tomcat_dev_hostkey} -i C:/Users/PURVISIS/Downloads/tomcat2.ppk webapp/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }
 
                stage ("Deploy to Production"){
                    steps {
                        bat "pscp -batch -hostkey ${params.tomcat_prod_hostkey} -i C:/Users/PURVISIS/Downloads/tomcat2.ppk webapp/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}