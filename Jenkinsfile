pipeline {
    agent any
    environment {
        VENV = "venv"
    }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Setup Python') {
            steps {
                sh 'python -m venv $VENV'
                sh '. $VENV/bin/activate && pip install --upgrade pip'
                sh '. $VENV/bin/activate && pip install -r requirements.txt'
            }
        }
        stage('Lint') {
            steps {
                sh '. $VENV/bin/activate && pip install flake8 && flake8 .'
            }
        }
        stage('Test') {
            steps {
                sh '. $VENV/bin/activate && python -m unittest discover -s tests'
            }
        }
        stage('Build') {
            steps { echo 'Python build completed successfully!' }
        }
    }
    post {
        always { sh 'rm -rf $VENV' }
        success { echo 'Build succeeded!' }
        failure { echo 'Build failed!' }
    }
}
