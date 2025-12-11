pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo '=== Build Stage: Creating virtual environment and installing dependencies ==='
                bat 'python --version'
                bat 'pip --version'
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
                // Remove start /b if you want to see full logs
                bat 'venv\\Scripts\\python python-app-deploy\\app.py'
            }
        }
        stage('Test Application') {
            steps {
                echo '=== Test Application Stage: Running tests after deployment ==='
                bat 'venv\\Scripts\\python test_app.py'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully! ✅'
        }
        failure {
            echo 'Pipeline failed. Check the logs for more details. ❌'
        }
    }
}


