

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
            slackSend(channel: '#test', color: 'good', message: "Build #${env.BUILD_NUMBER} succeeded: ${env.BUILD_URL}")
        }
        failure {
            slackSend(channel: '#test', color: 'danger', message: "Build #${env.BUILD_NUMBER} failed: ${env.BUILD_URL}")
        }
    }
}
