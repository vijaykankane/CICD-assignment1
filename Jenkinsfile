pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo 'Creating virtual environment and installing dependencies...'
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh '''
                    . venv/bin/activate
                    python -m pytest test_app.py -v
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying to staging...'
                sh '''
                    . venv/bin/activate
                    pkill -f "python app.py" || true
                    nohup python app.py > app.log 2>&1 &
                    sleep 5
                    curl -f http://localhost:5000/health || exit 1
                '''
            }
        }
    }
    
    post {
        success {
            emailext (
                subject: "Jenkins Build SUCCESS: ${env.JOB_NAME} - ${env.BUILD_NUMBER}",
                body: "Build completed successfully!\nBuild URL: ${env.BUILD_URL}",
                to: "your-email@example.com"
            )
        }
        failure {
            emailext (
                subject: "Jenkins Build FAILED: ${env.JOB_NAME} - ${env.BUILD_NUMBER}",
                body: "Build failed!\nBuild URL: ${env.BUILD_URL}",
                to: "your-email@example.com"
            )
        }
    }
}