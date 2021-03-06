# Scribbr

Weather prediction application

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

As the application runs in a docker container, these should be installed:

```
Composer
Docker
Docker-machine (MacOS, Windows)
Git
```

### Installing

Clone the project


```
git clone https://github.com/chicaum/scribbr.git
```

Change into project dir

```
cd scribbr
```

Install the dependencies

```
composer install
```

Start the container
```
docker-compose up -d
```

##Prepare the database

Execute a 'docker ps' command and get the fpm container ID
```
CONTAINER ID        IMAGE                                   COMMAND                  CREATED             STATUS              PORTS                               NAMES
4e34abb66618        scribbr_fpm                             "docker-php-entrypoi…"   6 minutes ago       Up 6 minutes        9000/tcp                            scribbr_fpm_1
25f71e8be24a        scribbr_nginx                           "nginx -g 'daemon of…"   6 minutes ago       Up 6 minutes        0.0.0.0:8011->80/tcp                scribbr_nginx_1
b9eac2e09f06        mysql:5.6                               "docker-entrypoint.s…"   About an hour ago   Up 6 minutes        0.0.0.0:3311->3306/tcp              scribbr_db_1
```

Execute the migrations command that will setup the database
```
docker exec <FPM-CONTAINER-ID> php bin/console doctrine:migrations:migrate --no-interaction
```
## Usage

At this point the database is empty.
It is possible to load fixtures into database or create and import data through the API
##Load Fixtures
Execute the load fixtures command that will populate the database
```
docker exec <FPM-CONTAINER-ID> php bin/console doctrine:fixtures:load --append
```

## Step 1 - Adding partners

Please use the endpoint `/admin/provider-add/{name}/{type}` to add new providers. It's easy!

Eg: Add the provider "BBC" with file type "csv"

`curl http://localhost:8011/admin/provider-add/bbc/csv`

Also, the data from partners must be mocked, please add a new folder for each partner and put the files inside:

/src/Integration/Data/<provider-name>  The folder name must be with lowercases 

## Step 2 - Integration of partners data

Please use the endpoint `/integrator/{partner-id}/{type}` to import a data file

Eg: Importing data from partner "BBC" with file type "csv"

`curl http://localhost:8011/integrator/1/csv`


## Access the application

```
http://localhost:8011
```

## Database access - MySQL 5.6

```
user: root
pass: root
port: 3311
```
 
## Running the tests

```
./bin/phpunit

#!/usr/bin/env php
PHPUnit 6.5.13 by Sebastian Bergmann and contributors.

Testing Project Test Suite
...........                                                       11 / 11 (100%)

Time: 234 ms, Memory: 4.00MB

OK (11 tests, 41 assertions)
```

## Built With

* [Docker](https://www.docker.com/) - Docker - Build, Ship, and Run Any App, Anywhere
* [Symfony](https://symfony.com/) - Symfony, High Performance PHP Framework for Web Development
* [Composer](https://getcomposer.org/) - Dependency Management

## Authors

* **Ademir Francisco da Silva** - *Initial work* - [Ademir Silva](https://github.com/chicaum)
