def secrets = [
  [path: 'dev-dexco/data/docker/mysql', engineVersion: 2, secretValues: [
    [envVar: 'DB_PASSWORD_ROOT', vaultKey: 'genesis_dbpass_root'],
    [envVar: 'DB_USERNAME', vaultKey: 'genesis_dbpass_user'],
    [envVar: 'DB_PASSWORD_USER', vaultKey: 'genesis_dbpass_userpass']]],
]

def configuration = [vaultUrl: 'http://54.82.251.82:8200',  vaultCredentialId: 'vault-approle', engineVersion: 2]

pipeline {
agent any

    stages {     
      stage('BuildDockerSQL') {
          steps {
            withVault([configuration: configuration, vaultSecrets: secrets]) {
            sh  '''
               docker run --name mysql-pocvault \
                    --publish 3306:3306 \
                    --env MYSQL_ROOT_PASSWORD=$DB_PASSWORD_ROOT \
                    --env MYSQL_ROOT_HOST=% \
                    --env MYSQL_DATABASE=poc_vault \
                    --env MYSQL_USER=$DB_USERNAME \
                    --env MYSQL_PASSWORD=$DB_PASSWORD_USER \
                    --detach mysql/mysql-server:5.7
            '''
            }
          }
      }
    }
}