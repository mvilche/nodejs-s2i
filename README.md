# NodeJS S2I Images CentOS 8

![Docker Stars](https://img.shields.io/docker/stars/mvilche/nodejs-s2i.svg)
![Docker Pulls](https://img.shields.io/docker/pulls/mvilche/nodejs-s2i.svg)
![Docker Automated](https://img.shields.io/docker/cloud/automated/mvilche/nodejs-s2i)
![Docker Build](https://img.shields.io/docker/cloud/build/mvilche/nodejs-s2i)


# Features

- Non-root
- Openshift compatible
- S2i build images
- Quasar
- Yarn
- Sonarqube Scanner
- Npm nexus private repository

### Deploy Environments 


| Environment | Details |
| ------ | ------ |
| TIMEZONE | Set Timezone (America/Montevideo, America/El_salvador) |
| WAITFOR_HOST | set name host |
| WAITFOR_PORT | set port for WAITFOR_HOST |


### Build Environments 


| Environment | Details |
| ------ | ------ |
| SONARQUBE_ENABLED | Enable sonarqube scanner. 1/0  |
| SONARQUBE_HOST | Sonarqube host. http://sonar.domain:9090  |
| SONARQUBE_PROJECTNAME | Sonarqube project name  |
| SONARQUBE_PROJECTKEY | Sonarqube project key  |
| NEXUS_NPM_REPO | Url private composer repository  |






### How use in Openshift

```console

oc process -f https://raw.githubusercontent.com/mvilche/nodejs-s2i-openshift/master/nodejs-s2i-template-openshift.yaml \ 
-p PHP_VERSION=nodejs10 \
-p APP_NAME=miapp \ 
-p APP_REPO=https://github.com/myuser/nodejs-sample-app.git | oc create -f -

```



### Use configmap for database connection using composer env file

##### Create a file yaml composer-env.yaml with keyname "env"

```yaml

apiVersion: v1
data:
  env: |-
    APP_NAME=Lumen
    APP_ENV=local
    APP_KEY=
    APP_DEBUG=true
    APP_URL=http://miapp
    APP_TIMEZONE=America/Montevideo
    LOG_CHANNEL=stack
    LOG_SLACK_WEBHOOK_URL=
    DB_CONNECTION=mysql
    DB_HOST=mysql
    DB_PORT=3306
    DB_DATABASE=database_name
    DB_USERNAME=user
    DB_PASSWORD=pass
    CACHE_DRIVER=file
    QUEUE_CONNECTION=sync
kind: ConfigMap
metadata:
  name: nodejs-env

```

##### Create configmap in Openshift

```console

 oc create -f nodejs-env.yaml

```

##### IMPORTANT: Mount configmap in Pod /opt/composer_env




### Generate builder image

```console

 docker build -t nodejs-s2i:10 -f nodejs10/Dockerfile.centos8 nodejs10

```

### Php application image use s2i

```console

s2i build https://github.com/my_phpapp.git nodejs-s2i:10 my_app:latest --incremental

```


### Run application

```console

docker run -p 8080:8080 my_app:latest

```

### How use s2i

```console

https://github.com/openshift/source-to-image

```

License
----

Martin vilche
