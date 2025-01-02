pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven 'M3'
    }

    stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/nikishkaa/jenkins-test.git']])
                sh 'mvn clean install'
            }
        }
        stage('Build Docker Image'){
            steps{
                script{
                    sh 'docker build -f Dockerfilev2 -t nikishka/jenkin-test-v2 . '
                }
            }
        }
        stage('Push Image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                       sh 'docker login -u nikishka -p ${dockerhubpwd}'
}

                        sh 'docker push nikishka/jenkin-test-v2'
                }
            }
        }
    }
}

