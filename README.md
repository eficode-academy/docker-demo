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

### ping in a detached container

Run a container that does something in the background

```sh
docker run -d alpine ping -C 10 localhost
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
   

4. Run container with Dockerfile.

    ```shell
    docker run -d nginx-DF -p 8080:80 .
    ```

5. Check what is displayed on `http://localhost:8080`


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