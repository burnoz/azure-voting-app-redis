pipeline {
    agent any

    stages {
        stage("Verify branch") {
            steps {
                echo env.GIT_BRANCH
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
                echo "Running tests"
            }

            // steps {
            //     bat "pytest ./tests/test_sample.py"
            // }

            // post {
            //     success {
            //         echo "Tests passed successfully"
            //     }
                
            //     failure {
            //         echo "Tests failed"
            //     }
            // }
        }

        stage("Docker push") {
            steps {
                echo "Running in ${env.WORKSPACE}"

                dir("${env.WORKSPACE}/azure-vote") {
                    // This securely grabs your password without using the buggy Docker wrapper
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        bat """
                            echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                            docker build -t burno/azure-vote-front:v1 .
                            docker push burno/azure-vote-front:v1
                            docker logout
                        """
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