pipeline {
    agent any

    environment {
        REACT_IMAGE = "react_app:latest"
        NODE_IMAGE = "node_app:latest"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/dung-mai/react-node-jenkins.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    dir('react-app') {
                        sh "docker build -t $REACT_IMAGE ."
                    }
                    dir('node-app') {
                        sh "docker build -t $NODE_IMAGE ."
                    }
                }
            }
        }

        stage('Run Containers') {
            steps {
                script {
                    sh "docker run -d -p 3000:3000 $REACT_IMAGE"
                    sh "docker run -d -p 5000:5000 $NODE_IMAGE"
                }
            }
        }

        stage('Test Application') {
            steps {
                script {
                    sh "curl -f http://localhost:3000 || exit 1"
                    sh "curl -f http://localhost:5000 || exit 1"
                }
            }
        }
    }

    post {
        always {
            script {
                sh "docker container prune -f"
                sh "docker image prune -f"
            }
        }
        success {
            echo 'Build and run successful!'
        }
        failure {
            echo 'Build or run failed!'
        }
    }
}
