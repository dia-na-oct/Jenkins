

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
        stage('Build') {
    steps {
        script {
            bat './gradlew build'
        }
    }
}
             stage('sonar') {
            steps {
                withSonarQubeEnv('sonar') {
                    bat './gradlew sonar '
                }
            }
        }
    }
}
