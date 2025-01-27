pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *')
    }

    environment {
        WEB_PORT="${params.WEB_PORT}"
        DB_PORT="${params.DB_PORT}"
        VOLUMES_PATH="${params.VOLUMES_PATH}"
        NOTIFICATIONS_PATH="${params.NOTIFICATIONS_PATH}"
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

        stage('Build docker-compose.yml') {
            steps {
                script {
                    sh "DEVICES='${params.DEVICES}' envsubst '\$DEVICES' < docker-compose.yml.tpl > docker-compose.yml"
                }
            }
        }

        stage('Build images and start containers') {
            steps {
                script {
                    sh 'docker compose up -d'
                }
            }
        }
    }
}
