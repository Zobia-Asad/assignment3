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
                docker { 
                    image 'joyzoursky/python-chromedriver:3.9'
                    args '-u root'
                }
            }
            steps {
                sh 'pip install -r requirements.txt'
                sh 'pytest test_app.py --junitxml=test-results.xml'
            }
            
            // --- MOVED INSIDE THE STAGE ---
            post {
                always {
                    // Now it looks in the correct folder (@2)
                    junit 'test-results.xml'
                }
            }
        }
    }

    post {
        always {
            // Email the collaborator (Assignment Requirement)
            emailext body: "Job '${env.JOB_NAME}' - Build #${env.BUILD_NUMBER}\nStatus: ${currentBuild.currentResult}\nCheck console for details.",
                     subject: "Jenkins Build ${currentBuild.currentResult}: ${env.JOB_NAME}",
                     to: 'qasimalik@gmail.com' 
        }
    }
}