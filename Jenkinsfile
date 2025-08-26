pipeline {
    agent any

    environment {
        SF_INSTANCE = 'https://login.salesforce.com'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-creds',
                    url: 'https://github.com/your-org/your-repo.git'
            }
        }

        stage('Authenticate Salesforce') {
            steps {
                withCredentials([
                    string(credentialsId: 'sf-client-id', variable: 'SF_CLIENT_ID'),
                    string(credentialsId: 'sf-username', variable: 'SF_USERNAME'),
                    file(credentialsId: 'sf-jwt-key', variable: 'SF_JWT_KEY')
                ]) {
                    sh """
                        echo 'Authenticating with Salesforce...'
                        sf auth:jwt:grant \
                            --clientid $SF_CLIENT_ID \
                            --username $SF_USERNAME \
                            --jwtkeyfile $SF_JWT_KEY \
                            --instanceurl $SF_INSTANCE
                    """
                }
            }
        }

        stage('Validate Deployment') {
            steps {
                sh """
                    echo 'Running validation...'
                    sf project deploy preview --source-dir force-app --target-org $SF_USERNAME --json
                """
            }
        }

        stage('Deploy to Salesforce') {
            steps {
                sh """
                    echo 'Deploying to Salesforce...'
                    sf project deploy start --source-dir force-app --target-org $SF_USERNAME --wait 10 --verbose
                """
            }
        }
    }
}
