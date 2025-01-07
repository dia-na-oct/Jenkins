

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
      stage('sonarqube') {
            steps {
                withSonarQubeEnv('sonar') {
                    bat './gradlew sonar '
                }
            }
        }
    /*    stage('Quality Gate') {
            steps {
                script {
                    withSonarQubeEnv('sonarqube') {
                        timeout(time: 1, unit: 'MINUTES') {
                            def qg = waitForQualityGate()
                            if (qg.status != 'OK') {
                                error "Pipeline aborted due to quality gate failure: ${qg.status}"
                            }
                        }
                    }
                }
            }
        }*/
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
   stage('Deployy') {
            steps {
                echo 'Deploying to MyMavenRepo...'
                bat "./gradlew publish"
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
                archiveArtifacts artifacts: 'build/**/*', fingerprint: true
        }
        failure {
            slackSend(channel: '#test', color: 'danger', message: "Build hhhh #${env.BUILD_NUMBER} failed: ${env.BUILD_URL}")
        }
    }
}
