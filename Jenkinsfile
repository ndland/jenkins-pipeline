pipeline {
    environment {
        registry = "ndland/jenkins-docker-test"
        DOCKER_PWD = credentials('docker-login-pwd')
    }
    agent {
        docker {
            image 'ndland/node-docker'
            args '-p 3000:3000'
            args '-w /app'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage("Build"){
            steps {
                sh 'npm install'
            }
        }
        stage("Test"){
            steps {
                sh 'npm test'
            }
        }
        stage('Deploy and smoke test') {
          steps {
            sh './jenkins/scripts/deploy.sh'
          }
        }
        stage('Cleanup') {
          steps {
            sh './jenkins/scripts/cleanup.sh'
          }
        }
    }
}