pipeline {
    agent any

    environment {
        WORKSPACE_DIR = "${env.WORKSPACE}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Python Build & Test') {
            steps {
                // Run the Python build inside python:3.9-alpine container
                sh """
                docker run --rm \\
                    -v ${WORKSPACE_DIR}:/workspace \\
                    -w /workspace \\
                    python:3.9-alpine sh -c "
                        python -m venv venv && \\
                        . venv/bin/activate && \\
                        pip install --upgrade pip && \\
                        pip install flake8 && \\
                        flake8 . && \\
                        python -m unittest discover -s tests
                    "
                """
            }
        }

        stage('Build Complete') {
            steps {
                echo 'Python build completed successfully!'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up virtual environment (if any)...'
            sh 'rm -rf venv || true'
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
