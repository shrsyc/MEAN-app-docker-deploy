pipeline {
    agent any
    stages {
        stage('Build Backend & Docker Image') {
            steps {
                dir('backend') {
                    sh """
                        docker rmi -f shrsyc/dd_mean_backend || true
                        docker build -t shrsyc/dd_mean_backend .
                    """
                }
            }
        }
        stage('Build Frontend & Docker Image') {
            steps {
                dir('frontend') {
                    sh """
                        docker rmi -f shrsyc/dd_mean_frontend || true
                        docker build -t shrsyc/dd_mean_frontend .
                    """
                }
            }
        }
        stage('Run Docker Compose') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'db-creds', usernameVariable: 'DB_USER', passwordVariable: 'DB_PASS')]) {
                    sh '''
                        export DB_USER=$DB_USER
                        export DB_PASS=$DB_PASS
                        docker-compose down || true
                        docker-compose up -d
                    '''
                }
            }
        }

    }
}
