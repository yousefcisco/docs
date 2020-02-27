# How to start

## Prerequisites

For more details on each please see [A-requirements.md](/docker/A-requirements.md).

### Docker and Docker-compose

Docker and docker-compose must be installed (latest)

### Composer Authentification

`COMPOSER_AUTH` must be set in order to fetch sources from Magento 2 repository.
You may use your own credentials, or project specific (**important for working on Commerce (Enterprise) version** ).

`export COMPOSER_AUTH='{"http-basic":{"repo.magento.com": {"username": "", "password": ""}}}'`

You can obtain credentials using [Magento2 Marketplace](https://account.magento.com/applications/customer/login/)

**It is important to execute `export` in the same terminal window you will run `docker-compose up`**
or add it to the .bashrc or corresponding file and reload the terminal window or run `source ~/.bashrc`, depending on your shell and configuration. 

### Running on Production

Execute `docker-compose -f docker-compose.yml -f docker-compose.remote.yml up -d`

### Running locally

1.  Make sure [requirements](https://docs.scandipwa.com/#/docker/A-requirements) are met
2.  Clone the repository
```console
git clone git@github.com:scandipwa/scandipwa-base.git
```
3.  Set `COMPOSER_HOME` on your machine (you can obtain credentials using [Magento2 Marketplace](https://account.magento.com/applications/customer/login/))
```console
export COMPOSER_AUTH='{"http-basic":{"repo.magento.com": {"username": "REPLACE_THIS", "password": "REPLACE_THIS"}}}'
```

4.  Generate selfsigned ssl certificates with (more details [here](https://docs.scandipwa.com/#/docker/G-SSL-container) )
```console
make cert
```
5. Add the Certificate Authority to your computer's trusted certificates. You can find the generated certificates under `/opt/cert/`. On a mac you can trust the certificate authority by opening `/opt/cert/scandipwa-ca.pem` and setting it to Always Trust

6.  Pull and run the infrastructure
```console
docker-compose -f docker-compose.yml -f docker-compose.local.yml -f docker-compose.ssl.yml pull
``` 
```console
docker-compose -f docker-compose.yml -f docker-compose.local.yml -f docker-compose.ssl.yml up -d
```

> **NOTICE**: Do the following steps only in case you need ScandiPWA DEMO

7.  Stop the application container 
```console
docker-compose stop app
```
8.  Recreate existing database 
```console
docker-compose exec mysql mysql -u root -pscandipwa -e "DROP DATABASE magento; CREATE DATABASE magento;"
```
9.  Import DEMO ScandiPWA database: 
```console
docker-compose exec -T mysql mysql -u root -pscandipwa magento < deploy/latest.sql
```
10.  Recreate Docker infrastructure
```console
docker-compose -f docker-compose.yml -f docker-compose.local.yml -f docker-compose.ssl.yml up -d --force-recreate
```

11. Check out your new instance [https://scandipwa.local/](https://scandipwa.local/)

     If you want to change your base URL update the `.application` file and re-run the command in step 9

For more details please refer to [Development environment](docker/E-development-environment.md) section.
