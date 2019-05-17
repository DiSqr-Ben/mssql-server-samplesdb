# mssql-server-samplesdb

This project will create a docker image with all the sample databases restored.



## How to create the image

You can create the image with the following command:

```cmd
docker-compose build
```

## How to run the container

```cmd
docker-compose up --build
```
_With the --build you are first building the image, and then instantiating the container_

## How to change the SQL Server base image

The [Dockerfile](./Dockerfile) specifies which base SQL Server Instance you want to use for your image. 

In case you want to change the version of the SQL Server used, please go edit the first line of the [Dockerfile](./Dockerfile)  and select your prefered version. For example

Change 
```docker
FROM mcr.microsoft.com/mssql/server:2019-CTP2.5-ubuntu
```
To
```docker
FROM mcr.microsoft.com/mssql/server:2017-latest-ubuntu
```
To get the latest SQL Server 2017 version with applied CU

__NOTE:__ To see which SQL Server versions, please go [here](https://hub.docker.com/_/microsoft-mssql-server) and select your "tag"

## How to add new databases to the image

It´s as easy as modifying the [Dockerfile](./Dockerfile), and adding the new backups you want to restore, and modifying the [setup.sql](./setup.sql) file with the RESTORE command.

## How to change the sa password

The password for the "sa" account is specified at the [docker-compose.yml](./docker-compose.yml) file.

## How to manually run the container without docker-compose

To manually run the container without using the docker-compose, you must follow this:

### Create the image

```powershell
docker build -t myImage . 
```

#### Optional pull the image if it exists

```powershell
docker pull hub.docker.com/enriquecatala/mssql-server-samplesdb:2019-ctp2.5-ubuntu
```

### Run the container

Execute the container exposing port 14333 and connecting the local volue d:/ inside the container. This is very interesting when you want to get data from your local storage outside the container

```powershell
docker run -p 14333:1433 myImage  -it -v d:/:/data  -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<YourStrong!Passw0rd>'
```

#### Connect to the container

```powershell
sqlcmd -S <ip_address>,14333 -U SA -P '<YourNewStrong!Passw0rd>'
```