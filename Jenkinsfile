pipeline {
    agent any
    environment {
        SERVICE_NAME = 'spring-petclinic'
    }
    tools{
        maven "maven"
        jdk "jdk-17.35"
    }
    stages {
        stage('verify') {
            when {
                branch 'main'
            }
            environment{
              MAVEN_IMAGE='maven:3.8.3-openjdk-17'
            }
          
            steps {
                script {
                         docker.image("${MAVEN_IMAGE}").withRun('-v $HOME/.m2:/root/.m2') {
                            // artifacts are not versioned. using docker tags instead.
                            sh 'mvn clean verify -B -U'
                        }
                    
                }
            }
        }
        stage('Quality Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('build-feature') {
            when {
                branch 'feature/DAS-*'
            }
            environment{
              MAVEN_IMAGE='maven:3.8.3-openjdk-17'
            }
          
            steps {
                script {
                         docker.image("${MAVEN_IMAGE}").withRun('-v $HOME/.m2:/root/.m2') {
                            // artifacts are not versioned. using docker tags instead.
                            sh 'mvn -X dockerfile:build'
                        }
                }
            }
        }
        stage('build-main') {
            when {
                branch 'main'
            }
            environment{
              MAVEN_IMAGE='maven:3.8.3-openjdk-17'
            }
          
            steps {
                script {
                         docker.image("${MAVEN_IMAGE}").withRun('-v $HOME/.m2:/root/.m2') {
                            // artifacts are not versioned. using docker tags instead.
                            sh 'mvn -X dockerfile:build'
                        }
                }
            }
        }
    }
}
