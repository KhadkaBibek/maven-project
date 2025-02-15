pipeline{
   
environment {
    NAME = 'Bibek'
}  
parameters {
    choice choices:['dev','prod'], name:'select_environment'
} 
    stages{
        stage('build'){
            steps{
            sh 'mvn clean package -DskipTests=true'
           
        }}
    
    stage('test'){
        parallel{
            stage('testA'){
                agent{
                    label 'master_agent'
                }
                steps{
                    echo 'This is test A'
                    sh 'mvn test'
                }
            }
            stage('testB')
            {
                agent{
                    label 'master_agent'
                }
                steps{
                    echo 'This is test B'
                    sh 'mvn test'
            }
            }
        }
    }
    }
    post{
        success{
            archiveArtifacts artifacts: '**/target/*.war'
        }
    }
}
