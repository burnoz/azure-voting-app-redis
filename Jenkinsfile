pipeline {
    agent any

    stages {
        stage("Verify branch") {
            steps {
                echo GIT_BRANCH
            }
        }

        stage("Docker build") {
            steps {
                bat "docker compose build"
            }
        } 

        stage("Start app") {
            steps {
                bat "docker compose up -d"
            }
        }

        stage("Run tests") {
            steps {
                bat "pytest ./tests/test_sample.py"
            }

            post {
                success {
                    echo "Tests passed successfully"
                }
            }

            post {
                failure {
                    echo "Tests failed"
                }
            }
        }
    }

    post {
        always {
            bat "docker compose down"
            echo "Docker compose down executed"
        }
    }
}