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
    }
}