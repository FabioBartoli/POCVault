pipeline {
agent any
    environment {
        VAULT_ADDR="http://54.82.251.82:8200/"
        // ROLE_ID="c4cec819-eae2-ca98-b312-46fcfe322c7c"
        // SECRET_ID=credentials("SECRET_ID")
        // SECRETS_PATH="secrets/creds/dev"
    }

    stages {     
      stage('BuildDockerSQL') {
          steps {
            sh """
            export DB_PASSWORD_ROOT=$(vault kv get -field=genesis_dbpass_root dev-dexco/docker/mysql)
            export DB_USERNAME=$(vault kv get -field=genesis_dbpass_user dev-dexco/docker/mysql)
            export DB_PASSWORD_USER=$(vault kv get -field=genesis_dbpass_userpass dev-dexco/docker/mysql)


            docker run --name mysql-pocvault \
                    --publish 3306:3306 \
                    --env MYSQL_ROOT_PASSWORD=$DB_PASSWORD_ROOT \
                    --env MYSQL_ROOT_HOST=* \
                    --env MYSQL_DATABASE=poc_vault \
                    --env MYSQL_USER=$DB_USERNAME \
                    --env MYSQL_PASSWORD=$DB_PASSWORD_USER \
                    --detach mysql/mysql-server:5.7
            """
          }
      }
    }
}