pipeline{
agent any
stages{
stage('build'){
steps{
sh './gradlew build'
archiveArtifacts 'build/libs/*.jar'
}
}
}
}