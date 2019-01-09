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
            stage ('Deploy to Stagint'){
                steps {
                    build job: 'Deploy-to-staging'
                }
            }
        }
    }
}