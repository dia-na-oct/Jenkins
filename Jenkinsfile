

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
 
             stage('sonar') {
            steps {
                withSonarQubeEnv('sonar') {
                    bat './gradlew sonar '
                }
            }
        }
stage('Build') {
                      steps {
                          script {
                              bat './gradlew jar'
                              bat './gradlew javadoc'
                          }
                      }
                      post {
                          success {
                              archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
                              archiveArtifacts artifacts: 'build/docs/javadoc/**/*', fingerprint: true
                          }
                      }
                  }
    }
}
