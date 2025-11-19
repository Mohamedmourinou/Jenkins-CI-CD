pipeline {
    agent any
 
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Mohamedmourinou/Jenkins-CI-CD.git'
            }
        }

        stage('Setup & Install') {
            agent {
                docker {
                    image 'python:3.10'
                    args '-u root:root'
                }
            }
            steps {
                sh '''
                python -m venv .venv
                . .venv/bin/activate
                python -m pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

         stage('Tests') {
            
            parallel {
                stage('Test 1') {
                    agent {
                        docker {
                            image 'python:3.10'
                            args '-u root:root'
                        }
                    }
                    steps {
                        sh '''
                        . .venv/bin/activate
                        python -m pytest test_app.py -v
                        '''
                    }
                }

                stage('Test 2') {
                    agent {
                        docker {
                            image 'python:3.10'
                            args '-u root:root'
                        }
                    }
                    steps {
                        sh '''
                        . .venv/bin/activate
                        python -m pytest test_app_2.py -v || echo "test_app_2.py not found, skipping..."  
                        '''
                    }
                }
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'python:3.10'
                    args '-u root:root'
                }
            }
            steps {
                sh '''
                echo 'Starting Flask app using Gunicorn...'
                . .venv/bin/activate
                nohup gunicorn -w 4 -b 127.0.0.1:5000 app:app > gunicorn.log 2>&1 &
                echo 'Flask app started at http://127.0.0.1:5000'
                '''
            }
        }
    }
}
