pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *')
    }

    stages {
        stage('Stop containers and remove images') {
            steps {
                script {
                    if (fileExists('docker-compose.yml')) {
                        sh 'docker compose down --rmi all'
                    }
                }
            }
        }

        stage('Checkout source code') {
            steps {
                git branch: 'main', url: 'https://github.com/NeverWise/scrutiny-docker-config.git'
            }
        }

        stage('Build images and start containers') {
            steps {
                script {
                    DEVICES="${params.DEVICES}"
                    WEB_PORT="${params.WEB_PORT}"
                    DB_PORT="${params.DB_PORT}"
                    VOLUMES_PATH="${params.VOLUMES_PATH}"
                    NOTIFICATIONS_PATH="${params.NOTIFICATIONS_PATH}"
                    NETWORK_NAME="${env.NETWORK_NAME}"
                    sh "envsubst '$DEVICES' < docker-compose.yml | docker compose -f - up -d"
                }
            }
        }
    }
}
