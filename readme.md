# Docker Symfony (PHP-FPM, Nginx, PostgreSQL, Adminer, Workspace)

### Prerequisites

Make sure these dependencies are installed on you machine

- [Docker](https://www.docker.com/ "Docker")
- [Docker compose](https://docs.docker.com/compose/ "Docker compose")

Docker containers primary based on [Laradock containers](https://github.com/laradock/laradock). For more details you can visit [Laradock documentation](http://laradock.io/) site

### Installation

* Clone the project
```bash
$ git clone git@github.com:eroslover/docker-symfony.git
```

* Create a `.env` from the `env-example` file. It will be the main env file. You can change it according to your application
```bash
$ cp env-example .env
```

* It is recommended to use different configurations of docker-compose.yml file for local, dev and prod environments.
You can create `docker-compose.local.yml` or `docker-compose.dev.yml` from the `docker-compose.yml` file

* Create Symfony project in your application folder. 
All the necessary information about the installation can be found in the official [framework documentation](https://symfony.com/doc/current/setup.html).
For example you can use this command:
```bash
$ composer create-project symfony/website-skeleton symfony
```
After installing the application folder structure must be like this:
```$xslt
Application Folder
    - docker (folder with docker containers and their data)
    - symfony (your project folder)
```
Then copy all necessary variables from symfony env file to the main env file.

### Start local environment

* To run local environment execute the following command from the root of the project:
`docker-compose -f docker-compose.local.yml up -d`. If there is no previously built images then 
docker will take from 5 to 10 minutes to download and build images. Then it will start containers. 
To verify containers were started correctly run `docker-compose -f docker-compose.local.yml ps`.

### Using PostgreSQL with Symfony 4

* Edit `config/doctrine.yaml` and replace the default mysql settings in `doctrine/dbal` section as follows
doctrine:
```
dbal:
    driver: 'pdo_pgsql'
    charset: utf8
```

## How to enable XDebug

* Set the variable `PHP_FPM_INSTALL_XDEBUG` to `true` in the main env file
* Set the variable `DOCKER_HOST_IP` to your machine IP address (`ifconfig` or such)
* Rebuild php-fpm docker container `docker-compose -f docker-compose.local.yml up -d --no-deps --build php-fpm`
* Don't forget to configure server in your IDE

## Useful commands

```$xslt
# Enter to workspace bash for using composer, npm or symfony commands
$ docker-compose -f docker-compose.local.yml exec --user symfodock workspace bash

# Stop docker containers
$ docker-compose -f docker-compose.local.yml stop

# Stop and delete docker containers
$ docker-compose -f docker-compose.local.yml down

# Clear cache
$ php app/console cache:clear # Symfony2
$ php bin/console cache:clear # Symfony3, 4

# Delete all containers
$ docker rm -f $(docker ps -aq)

# Delete all images
$ docker rmi -f $(docker images -aq)
```