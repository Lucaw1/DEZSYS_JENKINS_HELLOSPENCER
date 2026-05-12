pipeline {
    agent any
    
    environment {
        APP_PORT = '5556'
        GITHUB_REPO = 'https://github.com/Lucaw1/DEZSYS_JENKINS_HELLOSPENCER.git'
    }
    stages {
        stage('Checkout') {
            steps {
                cleanWs()
                git branch: 'main', url: "${GITHUB_REPO}"
            }
        }
        stage('Build') {
            steps {
                sh '''
                    python3 -m pip install --upgrade pip --break-system-packages
                    pip3 install -r requirements.txt --break-system-packages
                    if [ ! -f count.txt ]; then
                       echo "0" > count.txt
                    fi
                    chmod 666 count.txt
                '''
            }
        }
        stage('Test') {
            steps {
                sh 'python3 -m pytest tests/test_hello.py -v'
            }
        }
        stage('Run') {
            steps {
                sh '''
                    nohup python3 src/hello.py > app.log 2>&1 &
                    sleep 5
                    curl http://localhost:5556/api/hello
                '''
            }
        }
        stage('Test API') {
            steps {
                sh 'python3 -m pytest tests/test_api.py -v'
            }
        }
    }
    post {
        always {
            sh 'pkill -f "python3 src/hello.py" || true'
        }
    }
}