pipeline {
    environment {
        registry = "zakariaaz/tp4-app"
        registryCredential = '40068e42-aa01-45b4-8b58-b6350bf2f3b1'
        dockerImage = ''
    }
    agent any
    
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main',
                    credentialsId: 'eb1c4488-bb0c-475d-bdf7-c379ed4419e8',
                    url: 'https://github.com/AzmiZakaria/tp4devops.git'
            }
        }
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Test image') {
            steps {
                script {
                    sh "docker run --rm ${registry}:${BUILD_NUMBER} nginx -t"
                    echo 'Tests passed ✓'
                }
            }
        }
        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
}