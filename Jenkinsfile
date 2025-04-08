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
                    sh '''
                        docker-compose down -v || true
                        docker-compose up -d
                    '''
            }
        }

    }
}
