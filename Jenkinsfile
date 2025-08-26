pipeline {
    agent any

    environment {
        SF_CLIENT_ID = credentials('sf-client-id')   // store as Jenkins Secret Text
        SF_JWT_KEY   = credentials('sf-jwt-key')     // store as Jenkins Secret File
        SF_USERNAME  = 'your-username@company.com'
        SF_INSTANCE  = 'https://login.salesforce.com'  // or test.salesforce.com for sandbox
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/your-org/your-repo.git'
            }
        }

        stage('Authenticate to Salesforce') {
            steps {
                bat """
                echo Authenticating to Salesforce...
                sf force:auth:jwt:grant ^
                    --clientid %SF_CLIENT_ID% ^
                    --jwtkeyfile %SF_JWT_KEY% ^
                    --username %SF_USERNAME% ^
                    --instanceurl %SF_INSTANCE%
                """
            }
        }

        stage('Deploy to Salesforce') {
            steps {
                bat """
                echo Deploying metadata to Salesforce...
                sf project deploy start --source-dir force-app --wait 10 --verbose
                """
            }
        }
    }
}
