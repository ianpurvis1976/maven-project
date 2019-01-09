pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                bat 'wmic computersystem get name'
                bat 'echo %PATH%'
                echo bat(returnStdout: true, script: 'set')
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to Staging'){
            steps {
                build job: 'Deploy-to-staging'
            }

            build job: 'Deploy-to-Prod'
        }
        
        stage ('Deploy to Production'){
            steps {
                timeout(time:5, unit:'DAYS'){
                    input message: 'Approve Production Deployment?'
                }
                build job: 'Deploy-to-staging'
            }
        
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo 'Deployment failed.'
                }
            }
        }
    }
}