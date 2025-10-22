pipeline {
    agent {
        docker {
            image 'python:3.10'
            args '-u root:root' 
        }
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Mohamedmourinou/Jenkins-CI-CD'
            }
        }

        stage('Setup Venv') {
            steps {
                sh """
                python3 -m venv .venv
                source .venv/bin/activate
                python -m pip install --upgrade pip
                pip install -r requirements.txt
                """
            }
        }

        stage('Run Tests') {
            steps {
                sh """
                source .venv/bin/activate
                python3 -m pytest test_app.py -v
                """
            }
        }

        stage('Deploy') {
            steps {
                echo 'Starting Flask app locally (Linux agent in Docker)...'
                sh """
                export FLASK_APP=app
                source .venv/bin/activate
                nohup flask run --host=127.0.0.1 --port=5000 > flask.log 2>&1 &
                echo Flask started on http://127.0.0.1:5000
                """
            }
        }
    }
}
