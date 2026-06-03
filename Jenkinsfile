pipeline {
    agent any

    environment {
        IMAGE_NAME = "python-project"
        DOCKER_HUB = "reshma0209"
        BUILD_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Install & Test') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r app/requirements.txt
                # Fix module path issue
                export PYTHONPATH=$PWD
                pytest tests/
                '''
            }
        }

       stage('Build & Push Docker Image') {
    steps {
        withCredentials([usernamePassword(credentialsId: 'YOUR_DOCKER_CREDENTIALS_ID', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
            sh '''
            echo $PASS | docker login -u $USER --password-stdin
            
            // Highlight-start: Change 'reshma0209' to your login username 'nithya65'
            docker build -t nithya65/python-project:${BUILD_NUMBER} app/
            docker tag nithya65/python-project:${BUILD_NUMBER} nithya65/python-project:latest
            docker push nithya65/python-project:${BUILD_NUMBER}
            docker push nithya65/python-project:latest
            // Highlight-end
            '''
        }
    }
}
        stage('Deploy') {
            steps {
                echo "🚀 Deploy step (optional)"
            }
        }
    }

    post {
        success {
            echo "✅ Success: $DOCKER_HUB/$IMAGE_NAME:$BUILD_TAG"
        }
        failure {
            echo "❌ Failed"
        }
    }
}
