pipeline{
    agent {
        label 'server1'
    }
   
    stages{
        stage('build'){
            steps{
            sh 'mvn clean package'
        }}
    }
    post{
        success{
            archiveArtifacts artifacts: '**/target/*.jar'
        }
    }
}
