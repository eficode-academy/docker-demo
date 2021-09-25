# Docker Demos

---

## Hello world

Run the hello-world image to check that your docker installation works!

```sh
docker run hello-world
```

### echo & ls

Make docker say hello

```sh
docker run alpine echo "hello from docker!"
```

Show the file system of a container

```sh
docker run alpine ls
```

---

## Run some cool containers:

### Run an interactive shell in a container

Try some commands like `ls`, `whoami`, `cat /etc/os-release`

```sh
docker run -it alpine
```

### Containers are emphemeral, so lets try to delete the filesystem of a container!

Run an interactive ubuntu container and delete the entire filesystem!

**Be careful not to do this on your host machine!**

```sh
docker run -it ubuntu
rm -rf / --no-preserve-root
```

Try some commands like `echo`, `ls`, `bash` do some of them work? why?

Exit the container when you are done playing around.

Then run a new ubuntu container, which will start form the same immutable docker image, meaning we can break as many containers as we want, because we always just create new ones!

```sh
docker run -it ubuntu
```

### ping in a detached container

Run a container that does something in the background

```sh
docker run -d alpine ping -c 10 localhost
```

View running containers with

```sh
docker ps
```

View the stdout/stderr stream from container

```sh
docker logs <container id/name>
```

### Port forwarding with nginx

Run a container and forward a port from the host to the container, so that we can access it

```sh
docker run -d -p 80:80 nginx
```

Then access it on, e.g. `http://localhost` (make sure it is not trying to use https!)

### Bind-mount the host file system into a container

Create a directory and some html:

```sh
mkdir my_html
cd my_html
echo '<h1>Docker is cool!</h1>' > index.html
```

Run an nginx container and override the default index.html with our custom one:

```sh
docker run -d -p 8080:80 -v <path-to-your-my_html>/usr/share/nginx/html nginx
```

## Run container with the application

1. Run nginx container with port forwarding.
   
    ```sh
    docker run -d -p 8080:80 nginx
    ```

2. Create index.html file 
   
    ```sh
    <h1>Dockerfiles are awesome!</h1>
    ```

3. Create Dockerfile as follow

    ```sh
    FROM nginx
    COPY ./index.html /usr/share/nginx/html/index.html
    ```
   

4. Build image with Dockerfile.

    ```shell
    docker build -t nginx-test:1.0  .
    ```

5. Run docker image

    ```shell
    docker run -d -p 8080:80 nginx-test:1.0 
    ```

6. Check what is displayed on `http://localhost:8080`


## Docker-compose Nextcloud showcase

An example of starting a two-container application using docker-compose;  Nextcloud with a MariaDB backend.


1. Go to `docker-compose` directory, to start the application run:

    ```shell
    docker-compose up
    ```

2. Visit Nextcloud in a browser on: [localhost](http://localhost), to check if your Nextcloud is there.

3. Stop the services afterwards.
    If you ran `docker-compose` without `-d`, then `ctrl+c` will stop the services.

    ```shell
    docker-compose down
    ```
4. You can even remove the containers:

    ```shell
    docker-compose rm
    ```