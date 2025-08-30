stage('Deploy Code') {
    steps {
        withCredentials([file(credentialsId: 'your-jwt-key-cred-id', variable: 'jwt_key_file')]) {
            bat '''
            echo Authorizing with Salesforce...

            sf force:auth:jwt:grant ^
              --client-id 3MVG99AeQQhMVo3SAh7nBX_evEGcaF3nEdNhkHZ35.h2DnvCcEwXZBuqdPoAw1q7hOkUoSSJ08eAwHUOoS.jt ^
              --username ktdocsgendevorg@techkasetti.com ^
              --jwt-key-file %jwt_key_file% ^
              --instance-url https://login.salesforce.com ^
              --set-default-dev-hub

            echo Authentication Successful!

            sf project deploy start --source-dir force-app --wait 10
            '''
        }
    }
}
