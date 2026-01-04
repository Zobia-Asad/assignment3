pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            agent {
                // This docker image has Python & Chrome pre-installed
                docker { 
                    image 'joyzoursky/python-chromedriver:3.9'
                    args '-u root'
                }
            }
            steps {
                sh 'pip install -r requirements.txt'
                // Run tests and save results for Jenkins to display
                sh 'pytest test_app.py --junitxml=test-results.xml'
            }
        }
    }
    post {
        always {
            junit 'test-results.xml'
        }
    }
}