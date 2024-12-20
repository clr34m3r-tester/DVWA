pipeline {
    agent any

    environment {
          WEBHOOK_URL = credentials('WEBHOOK_URL')
          GIT_TOKEN = credentials('GIT_TOKEN')
      }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/clr34m3r-tester/DVWA.git', branch: 'master'
            }
        }
        stage('Repo-scan') {
            steps {
                script {
                    def repoUrl = env.GIT_URL
                    def repoName = env.JOB_NAME.split('/')[0]

                    sh """
                    curl -X POST "$WEBHOOK_URL" \
                    -H "Content-Type: application/json" \
                    -d '{
                          "repository": {
                            "clone_url": "${repoUrl}",
                            "name": "${repoName}"
                          },
                          "scanner": "gitleaks",
                          "parameters": [
                            "--verbose"
                          ],
                          "tokenSecret": "${GIT_TOKEN}"
                        }'
                    """
                }
            }
        }
        stage('Build') {
            steps {
                // Add your build steps here
                echo 'Building...'
            }
        }
        stage('Deploy') {
            steps {
                // Add your deploy steps here
                echo 'Deploying...'
            }
        }
    }
}
