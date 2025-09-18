pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo 'Installing dependencies...'
                sh 'pip3 install -r requirements.txt'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'python3 -m pytest test_app.py -v'
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying to staging...'
                sh '''
                    pkill -f "python3 app.py" || true
                    nohup python3 app.py > app.log 2>&1 &
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