pipeline {
    agent any

    stages {

        stage('Branch Info') {
            steps {
                echo "Running pipeline for branch: ${env.BRANCH_NAME}"
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Build running for $BRANCH_NAME"'
            }
        }

        stage('Test') {
            steps {
                sh '''
                if [ "$BRANCH_NAME" = "main" ]; then
                  echo "Production tests running"
                else
                  echo "Dev tests running"
                fi
                '''
            }
        }

        stage('Install HTTPD (Main Only)') {
            when {
                branch 'main'
            }
            steps {
                sh '''
                sudo yum install httpd -y
                sudo systemctl enable httpd
                sudo systemctl start httpd
                '''
            }
        }

        stage('Deploy HTML (Main Only)') {
            when {
                branch 'main'
            }
            steps {
                sh '''
                sudo rm -rf /var/www/html/*
                sudo cp index.html /var/www/html/index.html
                sudo systemctl restart httpd
                '''
            }
        }
    }
}
