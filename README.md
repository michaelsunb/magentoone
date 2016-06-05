## Before using
Docker containers are not "lightweight VMs" or VMs. They should be thought of something
like [processes][docker_process] but really not because it is much more complicated than that.

Please google `Containers are not VMs` to get a better understanding.

[Processes][docker_process] because of the Docker best practices, and that this
container is `bundled` with a lot of applications, which is not good practice.

Also please read the Docker best practices for [Containers should be ephemeral][docker_ephemeral].
Containers should be `ephemeral` meaning they should be short lived, and that this
container should not be used in production or anything else other than testing.

Please be aware that when your container is exited, all the data in the MySql database
and any editted files inside the container will start over from the beginning.
In other words any changes from inside the container will be lost.

### Motivation
To quickly start up a LAMP server with phpmyadmin. Magento is a plus but really you
can add any PHP application using
> docker run -it -p 80:80 -v /{path to your app}/:/var/www/html/ michaelsunb/magentoone /bin/bash

### What's bundled :
- Magento 1.9.2.4
- PhpMyAdmin
- Ubuntu 14.04 with shell access
- AMP (Apache (2.0) / Php (5.6) / Mysql)
- vim, curl, git, pwgen, wget, xdebug, composer and phpunit

### How to run Magento using Docker Hub
Run the container and interact with it
> docker run --rm -it --name anynameyouwant -p 80:80 michaelsunb/magentoone /bin/bash

Or you can have your own Magento at this folder `/home/docker/data/` and run
> docker run --rm -it --name anynameyouwant -p 80:80 -v /home/docker/data/:/var/www/html/ michaelsunb/magentoone /bin/bash

NOTE: `--rm` will remove the container named "anynameyouwant" upon exit. 
      You can omit it from the command if you choose.

Inside the container run
> service apache2 start

> service mysql start

NOTE: You will have to do this every time you start up or exec into a container.

Then once done go to `http://{your docker ip}/phpmyadmin`. The MySql username is
`root` and password is nothing. Log in and create a database called `magento`.
Now you can start your Magento installation by going to `http://{your docker ip}/index.php`

NOTE: You will have to do this every time you start up a new container.

### How to build & run :
Step 1 :
Get into your server with SHELL access. And run a git pull which follows
> git clone https://github.com/michaelsunb/magentoone.git .

NOTE: There is a tiny little dot in the end

This will get basic files for building our docker image. Then start the build using
> docker build -t {sitename}/magentoone .

NOTE: There is a tiny little dot in the end

Step 2 :
Follow the steps from [How to run Magento using Docker Hub][how_to_run] but instead of `michaelsunb/magentoone`
replace it with `{sitename}/magentoone`

[docker_process]: https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/#run-only-one-process-per-container
[docker_ephemeral]: https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/#containers-should-be-ephemeral
[how_to_run]: https://github.com/michaelsunb/magentoone#how-to-run-magento-using-docker-hub
