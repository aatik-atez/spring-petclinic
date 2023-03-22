pipeline {
    agent 
     {
         docker
          {
            //This exposes application through port 8081 to outside world
            args '-u root -v /var/run/docker.sock:/var/run/docker.sock  '
         }
    }

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
