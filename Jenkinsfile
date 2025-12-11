pipeline {
    agent any

    stages {
        stage('Check Environment') {
            steps {
                echo '=== Checking Python and PATH ==='
                bat 'python --version'
                bat 'where python'
                bat 'pip --version'
            }
        }

        stage('Build') {
            steps {
                echo '=== Build Stage: Creating virtual environment and installing dependencies ==='
                bat 'python -m venv venv'
                bat 'venv\\Scripts\\pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                echo '=== Test Stage: Running unit tests ==='
                bat 'venv\\Scripts\\python -m unittest discover -s .'
            }
        }

        stage('Deploy') {
            steps {
                echo '=== Deploy Stage: Deploying application ==='
                bat 'if not exist python-app-deploy mkdir python-app-deploy'
                bat 'copy /Y app.py python-app-deploy\\'
            }
        }

        stage('Run Application') {
            steps {
                echo '=== Run Application Stage: Running Flask app ==='
                // Use start /b to run in background without blocking pipeline
                bat 'start /b venv\\Scripts\\python python-app-deploy\\app.py > python-app-deploy\\app.log 2>&1'
            }
        }

        stage('Test Application') {
            steps {
                echo '=== Test Application Stage: Testing deployed app ==='
                bat 'venv\\Scripts\\python test_app.py'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check the logs for details.'
        }
    }
}



