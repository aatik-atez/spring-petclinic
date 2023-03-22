pipeline {
    agent any

    environment {
        SERVICE_NAME = 'spring-petclinic'
    }
    stages {
        stage('docker-master') {
            when {
                branch 'main'
            }
            environment{
              MAVEN_IMAGE='maven:3.8.3-openjdk-17'
            }
          
            steps {
                script {
                         docker.image('${MAVEN_IMAGE}').withRun('-v $HOME/.m2:/root/.m2') {
                            // artifacts are not versioned. using docker tags instead.
                            sh 'mvn clean install -B -U'
                        }
                    
                }
            }
        }
    }
}
