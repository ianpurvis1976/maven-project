pipeline {
    agent any
    stages{
        stages{'Build'{
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