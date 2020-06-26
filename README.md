# Symfony3.4-Docker

This is a fully functional docker-compose environment to test and develop a Symfony 3.4 application.<br>
Get rid of MAMP / XAMPP / WAMP and all that boring and difficult stuff, bring up everything in seconds!

## How to use

  - First of all install Docker from [Docker.com][docker]
  - Create also an account on [DockerHub][dockerhub]
  - When you're done, launch Docker on your Windows / Mac and sign-in with you account
  - Clone this repo in the ROOT folder of your Symfony project
  - You now need some basic configuration:
    - Open the `.env` file and replace `<put-here-your-name>` with a name of your choice
    - Open the `docker-compose.yml` file and replace `<put-here-your-name>` with the same name you provided above
    - `OPTIONAL` If you need it, change the timezone in `php/php.ini` by setting your timezone
    - Configure your parameters:
      - Open `app/config/parameters.yml` and set the values:
        - `database_host`: db
        - `database_port`: 3306
        - `database_name`: the name you just chose
        - `database_user`: dev
        - `database_password`: dev
    - Open a terminal and type the following command: `docker network create -d bridge <put-here-your-name>_symfony_dev`, replacing `<put-here-your-name>` with the name you chose in the previous steps.
    - If you did everything correctly, you should now see the hash of the newly created network.
    
## Time to deploy

  - Open a terminal and navigate into the `docker` folder of your Symfony Project
  - Build the compose network by typing `docker-compose build` (*INTERNET CONNECTION IS REQUIRED THE FIRST TIME*)
  - When it is finished, you can start your network by typing `docker-compose up`
    - You'll see the following 4 containers coming up:
      - `db_1`: this is the machine that's responsible of all your database data
      - `php_1`: this is the machine where all your files are
      - `phpmyadmin_1`: this is the machine that enables PhpMyAdmin for you
      - `nginx_1`: this is the Nginx machine, used as a reverse proxy pointing to your php machine   
  - At the startup, php will try to execute a full `composer install`, a `schema update` and a full `assets install`
    - If everything went good, you should see a message indicating that the `php_1` container is ready to handle connections
    - If your db is big, it may be possible that `php_1` tries to execute a script on that db, before the `db_1` machine has created all tthe required steps:
      - This can happen only the first time you brig up the project: if it happens, proceed as follow:
        - Wait for `db_1` to show the message `mysqld: ready for connections.`
        - Do a `Ctrl / Cmd + C` to stop the process
        - Type `docker-compose up` again. This time the initialization will be very fast and `php_1` won't have problems connecting 

## Environment OK, let's have some fun

  - You can access your symfony application by typing `http://localhost` on your browser (it's already defaulted on `app_dev.php`)
  - You can access `PhpMyAdmin` by typing `http://localhost:8081` on your browser

   [docker]: <https://www.docker.com>
   [dockerhub]: <https://hub.docker.com>
  
