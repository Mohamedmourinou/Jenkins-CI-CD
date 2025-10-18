pipeline {
    agent any
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
                python -m venv .venv
                call .venv\\Scripts\\activate
                python -m pip install --upgrade pip
                pip install -r requirements.txt
                """
            }
        }

        stage('Run Tests') {
            steps {
                sh """
                call .venv\\Scripts\\activate
                python -m pytest test_app.py -v
                """
            }
        }

        stage('Deploy') {
            steps {
                echo 'Starting Flask app locally (Windows agent) ...'
                sh """
                set FLASK_APP=app
                call .venv\\Scripts\\activate
                start /B python -m flask run --host=127.0.0.1 --port=5000 > flask.log 2>&1
                echo Flask started on http://127.0.0.1:5000
                """
            }
        }
    }
}
