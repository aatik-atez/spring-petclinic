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
          
            steps {
                script {
                    docker.withServer('tcp://127.0.0.1:2345') {
                         docker.image("${MAVEN_IMAGE}").withRun('-v $HOME/.m2:/root/.m2') {
                            // artifacts are not versioned. using docker tags instead.
                            sh 'mvn clean install -B -U'
                        }
                    }
                }
            }
        }
    }
}
