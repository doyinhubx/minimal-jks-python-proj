pipeline {
    agent any

    environment { VENV = "venv" }

    stages {
        stage('Checkout') { steps { checkout scm } }
        stage('Setup Python') {
            steps {
                sh '''
                python3 -m venv $VENV
                . $VENV/bin/activate
                pip install --upgrade pip flake8
                '''
            }
        }
        stage('Lint') { steps { sh '. $VENV/bin/activate && flake8 . || true' } }
        stage('Test') { steps { sh '. $VENV/bin/activate && python -m unittest discover -s tests' } }
        stage('Build Complete') { steps { echo 'Python build completed successfully!' } }
    }

    post { always { sh 'rm -rf venv' } }
}
