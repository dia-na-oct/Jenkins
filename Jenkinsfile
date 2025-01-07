

pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                script {

                    bat './gradlew test' 
               
                }
            }
            post {
                always {

                    junit 'build/test-results/test/*.xml'

                    cucumber 'reports/example-report.json'
                }
            }
        }
 
        
    }
     post {
            success {
            slackSend(channel: '#test', color: 'good', message: "Build hello #${env.BUILD_NUMBER} succeeded: ${env.BUILD_URL}")
                emailext(
            to: 'd_guitoun@esi.dz',
            subject: "Build #${env.BUILD_NUMBER} Succeeded",
            body: "Good news! The build succeeded. Check it here: ${env.BUILD_URL}"
        )
        }
        failure {
            slackSend(channel: '#test', color: 'danger', message: "Build hhhh #${env.BUILD_NUMBER} failed: ${env.BUILD_URL}")
        }
    }
}
