# Deployment

## Introduction

For deploying our API we use [railway](https://railway.app/) which is a platform as a service
that allows us to deploy our code from GitHub directly, any changes made to the main branch
will start a new deployment process on railway.

## Dockerfile

Railway deploys our code by runnig it inside a docker container.  

We use a `Dockerfile` to customize the container. We customize the container, because
standard railway containers don't have the dependencies we need like `GDAL` for geodjango.  

**Note:** In deployment we store our environment variables using railway variables, these variables get injected into our container during build time, to use them, they must be specified at the start of the Dockerfile after `From ubuntu:latest` with:

```docker
ARG EnvironmentVariable
```

Then set as env variable with:

```docker
ENV EnvironmentVariable=${EnvironmentVariable}
```  

## Supervise.conf

This is our [Supervisor](http://supervisord.org/index.html) config file. We mainly use supervisor to allow us to run multiple processes of our ASGI server [Daphne](https://github.com/django/daphne) on same port.  

Any supervisor conf files should be placed in `/etc/supervisor/conf.d/` which we do in the `Dockerfile`
```docker
COPY supervise.conf /etc/supervisor/conf.d/
```

**Note**: Supervisor runs `Daphne` as a background service which for some reason can't read our env variables that is why we read them from a `.env` file which was created in the Dockerfile.
```docker
RUN touch .env
RUN printenv  > .env
```

## Nginx.conf
This is our [Nginx](https://www.nginx.com/) config file. It is the recommended setup to use `Nginx` when using `Daphne` for better security and performance.  

Nginx conf should be placed in `/etc/nginx/nginx.conf` which we do in the `Dockerfile`
```docker
COPY nginx.conf /etc/nginx/nginx.conf
```

> **_IMPORTANT NOTE:_** If you look at nginx.conf you will find that nginx listens on port 80, the port number must be stored in railway variables for railway to know which port does our server listen on, the deployment won't work without this.

## start.sh
This is the script that runs when our Docker container starts. It runs database migrations, start supervisor and nginx and prints daphne and nginx log to stdout.

## Media Storage

As mentioned before, any changes made to the main branch will start a new deployment process, which means the container state is not persistent and that is why we need a third party app for media storage.  

Media files include: 

- User profile photos 
- Files uploaded on the community channels

Right now we are using [Blackblaze](https://www.backblaze.com/) for media storage.

---