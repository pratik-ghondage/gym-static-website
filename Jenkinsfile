pipeline {
    agent any

    environment {
        SERVER_IP      = '172.31.2.172'
        SSH_CREDENTIAL = 'Virginia'
        REPO_URL       = 'https://github.com/pratik-ghondage/gym-static-website.git'
        BRANCH         = 'main'
        REMOTE_USER    = 'ubuntu'
        REMOTE_PATH    = '/var/www/html'
    }

    stages {

        stage('Deploy to Target Server') {
            steps {
                sshagent([SSH_CREDENTIAL]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${SERVER_IP} 'mkdir -p ${REMOTE_PATH}'
                        scp -o StrictHostKeyChecking=no -r * ${REMOTE_USER}@${SERVER_IP}:${REMOTE_PATH}
                    """
                }
            }
        }

        stage('Restart Nginx') {
            steps {
                sshagent([SSH_CREDENTIAL]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${SERVER_IP} '
                            sudo systemctl restart nginx
                        '
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful'
        }
        failure {
            echo 'Deployment Failed'
        }
    }
}