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
                
                failure {
                    echo "Tests failed"
                }
            }
        }

        stage("Docker push") {
            steps {
                echo "Running in ${env.WORKSPACE}"

                dir("${env.WORKSPACE}/azure-vote") {
                    script {
                        docker.withDockerRegistry('', 'dockerhub') {
                            def image = docker.build("azure-vote-front:v1")
                            image.push()
                        }
                    }
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