pipeline {
    agent any

    environment {
        EC2_HOST = 'YOUR_EC2_PUBLIC_IP'   // Replace with actual IP
        DEPLOY_DIR = '/var/www/mywebsite'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Validate') {
            steps {
                sh '''
                    if [ ! -f index.html ]; then
                        echo "index.html not found!"
                        exit 1
                    fi
                    echo "Validation passed."
                '''
            }
        }

        stage('Deploy') {
            steps {
                sshagent(credentials: ['ec2-ssh-key']) {
                    sh '''
                        scp -o StrictHostKeyChecking=no index.html ubuntu@${EC2_HOST}:${DEPLOY_DIR}/index.html
                        ssh -o StrictHostKeyChecking=no ubuntu@${EC2_HOST} "sudo nginx -t && sudo systemctl reload nginx"
                    '''
                }
            }
        }
    }
}
