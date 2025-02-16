pipeline {
    agent {
        label 'master_agent'
    }
    environment {
        NAME = 'Bibek'
    }
    parameters {
        choice choices: ['master', 'agent'], name: 'select_environment'
    }
    stages {
        stage('build') {
            steps {
                sh 'mvn clean package -DskipTests=true'
                echo "hello $NAME ${params.select_environment}"
            }
        }

        stage('test') {
            parallel {
                stage('testA') {
                    agent {
                        label 'master_agent'
                    }
                    steps {
                        echo 'This is test A'
                        sh 'mvn test'
                    }
                }

                stage('testB') {
                    agent {
                        label 'master_agent'
                    }
                    steps {
                        echo 'This is test B'
                        sh 'mvn test'
                    }
                }
            }
            post {
                success {
                    dir("webapp/target/") {
                        stash name: "master-maven-build", includes: '*.war'
                    }
                }
            }
       
     }
     stage('master_deploy'){
        when { expression {params.select_environment == 'master'}
        beforeAgent true}
        agent {
            label 'master_agent'
        }
        steps{
            dir("/var/www/html")
            {
                unstash "master-maven-build"
            }
            sh """
            cd /var/www/html/
            jar -xvf webapp.war
            """
        }
     }



    }
}
