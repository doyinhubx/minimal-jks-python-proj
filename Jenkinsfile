pipeline {
    agent {
        docker {
            image 'python:3.9-alpine'
            args '-v /var/jenkins_home:/var/jenkins_home'
        }
    }

    environment {
        VENV = "venv"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python') {
            steps {
                sh '''
                python -m venv $VENV
                . $VENV/bin/activate
                pip install --upgrade pip
                pip install flake8
                '''
            }
        }

        stage('Lint') {
            steps {
                sh '''
                . $VENV/bin/activate
                flake8 .
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                . $VENV/bin/activate
                python -m unittest discover -s tests
                '''
            }
        }

        stage('Build') {
            steps {
                echo 'Python build completed successfully!'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up virtual environment...'
            sh 'rm -rf $VENV'
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
