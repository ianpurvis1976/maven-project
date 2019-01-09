pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                echo 'Now Archiving...'
                archiveArtificats artifact '**/target/*.war'
            }
        }
    }
}