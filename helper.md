# How to Use This Repo

## Requirements

- minimum 3~4GB memory setting for Docker
- local copy of `nurx-daga-dags` repo
- local .env file in `docker` directory. (can fetch from [1password](https://my.1password.com/vaults/q53fddt66zfb7dwjqsilk2i3ee/allitems/u2yh2okx2veshij66rpo56qk5m), named `Airflow MWAA .env`)

## To Bring Up the Env

```
cd docker
docker-compose up
```

It takes about 1min for the webserver to start, so we can open it on `localhost:8080`

username and password is `admin`, pass is `test`. (This is local env, so no password leak issue)


## Notes

Other some useful commands:
```
# only bring up mwaa container without restart postgres:
docker-compose up --build local-runner

# list the uprunning containers:
docker container ls

# stop the containers:


# remove the containers: (normally we don't need to remove)
docker container rm `docker container ls -a|grep mwaa|awk '{print $1}'`

# Rebuild the MWAA Image(normally we don't need to)
docker build --rm --compress -t amazon/mwaa-local:1.10 ./docker --no-cache

# Enter into the container and test python dependencies:
docker exec -it docker_local-runner_1 /bin/bash

# To make sure airflow variables are set properly through .env:
docker-compose config
```

## Common Errors:

```
Broken DAG: [/usr/local/airflow/dags/crm_databots_generic.py] Descriptors cannot not be created directly. If this call came from a _pb2.py file, your generated code is out of date and must be regenerated with protoc >= 3.19.0. If you cannot immediately regenerate your protos, some other possible workarounds are: 1. Downgrade the protobuf package to 3.20.x or lower. 2. Set PROTOCOL_BUFFERS_PYTHON_IMPLEMENTATION=python (but this will use pure-Python parsing and will be much slower). More information: https://developers.google.com/protocol-buffers/docs/news/2022-05-06#python-updates
```

need add `protobuf~=3.20.0` in requirements.txt.
