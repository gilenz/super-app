pipeline {
    agent {
        node {
            label 'jenkins-docker-slave' 
        }
    }
    stages {
        stage('build docker compose') {
            steps {
                sh "docker compose build"
            }
        }
        stage('start the application') {
            steps {
                sh "docker compose up -d"
            }
        }
        stage('test the application') {
            steps {
                sh "curl localhost:8080"
            }
        }
        stage('Deploy to docker hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', passwordVariable: 'docker_password', usernameVariable: 'docker_user')]) {
                    sh "docker login -u $docker_user -p $docker_password"
                    sh """
                    docker tag super-app-app:latest gilni/super-app:${BUILD_NUMBER}
                    docker push gilni/super-app:${BUILD_NUMBER}
                    """
                }
            }
        }
    }
}