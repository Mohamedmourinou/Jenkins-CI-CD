pipeline{
    agent any
    stages{

        stage(Checkout){
            steps{
                git branch: 'main',
                url:'https://github.com/Insaf-Badri/Jenkins-CI-CD.git'
            }
        }

        stage(Setup){
            steps{
                bat """
                python -m venv .venv

                call .venv\\Scripts\\activate
                python -m pip install --upgrade pip
                pip install -r requirements.txt
                """
            }
        }

        stage(Tests){
            when{
                not{
                    changeset ""**/README.md""
                }
            }
            parallel{
                stage(test1){
                    steps{
                         bat """
                        call .venv\\Scripts\\activate
                        python -m pytest test_app.py -v
                        """
                    }
                }

                stage(test2){
                    steps{
                         bat """
                        call .venv\\Scripts\\activate
                        python -m pytest test_app_2.py -v
                        """
                    }
                }
            }
        }

        stage(Deploy){
             steps {
                echo 'Starting Flask app locally using Gunicorn...'
            }
        }

    }

}
    
