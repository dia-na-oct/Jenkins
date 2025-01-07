/*pipeline{
agent any
stages{
stage('build'){
steps{
 bat './gradlew build'
}
}
}
}*/

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
}
