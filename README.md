# schema-epa

Liquibase scripts for creating the STORETW and WQX database schema objects in a Postgres database. 
They are used for the Water Quality Portal (WQP).

## Docker
Also included are Docker Compose scripts to create PostgreSQL and Liquibase containers for testing the scripts.

### Docker Network
A named Docker Network is required for local running of the containers. Creating this network allows you to run all of the WQP locally in individual containers without having to maintain a massive Docker Compose script encompassing all of the required pieces. (It is also possible to run portions of the system locally against remote services.) The name of this network is provided by the __LOCAL_NETWORK_NAME__ environment variable. The following is a sample command for creating your own local network. In this example the name is wqp and the ip addresses will be 172.25.0.x

```
docker network create --subnet=172.25.0.0/16 wqp
```

### Environment variables
In order to use the docker compose scripts, you will need to create a .env file in the project directory containing
the following (shown are example values):

```
POSTGRES_PASSWORD=<changeMe>

EPA_DATABASE_ADDRESS=<epa_database_host>
EPA_DATABASE_NAME=<epa_db>
EPA_DB_OWNER_USERNAME=<wqp_core>
EPA_DB_OWNER_PASSWORD=<changeMe>

EPA_SCHEMA_OWNER_USERNAME=<epa_owner>
EPA_SCHEMA_OWNER_PASSWORD=<changeMe>

WQX_SCHEMA_NAME=<wqx>
WQX_DUMP_SCHEMA_NAME=<wqx_dump>

STORETW_SCHEMA_NAME=<storetw>
STORETW_DUMP_SCHEMA_NAME=<storetw_dump>

NWIS_SCHEMA_NAME=<nwis>

WQP_SCHEMA_NAME=<wqp>
WQP_SCHEMA_OWNER_USERNAME=<wqp_core>
WQP_SCHEMA_OWNER_PASSWORD=<changeMe>

LOCAL_NETWORK_NAME=<wqp>

DB_IPV4=<172.25.0.2>
DB_PORT=<5437>
LIQUIBASE_IPV4=<172.25.0.11>

LIQUIBASE_VERSION=<3.6.3>
JDBC_JAR=<postgresql-42.2.5.jar>

CONTEXTS=ci
```
#### Environment variable definitions

* **POSTGRES_PASSWORD** - Password for the postgres user.

* **EPA_DATABASE_ADDRESS** - Host name or IP address of the PostgreSQL database.
* **EPA_DATABASE_NAME** - Name of the PostgreSQL database to create for containing the schema.
* **EPA_DB_OWNER_USERNAME** - Role which will own the database.
* **EPA_DB_OWNER_PASSWORD** - Password for the **EPA_DB_OWNER_USERNAME** role.

* **EPA_SCHEMA_OWNER_USERNAME** - Role will own the database objects.
* **EPA_SCHEMA_OWNER_PASSWORD** - Password for the **EPA_SCHEMA_OWNER_USERNAME** role.

* **WQX_SCHEMA_NAME** - Name of the schema to create for holding WQX objects.
* **WQX_DUMP_SCHEMA_NAME** - Name of the schema to create for holding WQX_DUMP objects.

* **STORETW_SCHEMA_NAME** - Name of the schema to create for holding STORETW objects.
* **STORETW_DUMP_SCHEMA_NAME** - Name of the schema to create for holding STORETW_DUMP objects.

* **NWIS_SCHEMA_NAME** - Name of the schema to create for holding NWIS objects.

* **WQP_SCHEMA_NAME** - Name of the schema for the WQP objects.
* **WQP_SCHEMA_OWNER_USERNAME** - Role which owns the WQP database objects.
* **WQP_SCHEMA_OWNER_PASSWORD** - Password for the **WQP_SCHEMA_OWNER_USERNAME** role.

* **LOCAL_NETWORK_NAME** - The name of the local Docker Network you have created for using these images/containers.

* **DB_IPV4** - The IP address in your Docker Network you would like assigned to the database container used for testing the Liquibase scripts.
* **DB_PORT** - The localhost port on which to expose the script testing database container.
* **LIQUIBASE_IPV4** - The IP address you would like assigned to the Liquibase runner container.

* **LIQUIBASE_VERSION** - The version of Liquibase to install.
* **JDBC_JAR** - The jdbc driver to install.

* **CONTEXTS** - use "ci" to create a continuous integration database, otherwise leave empty.

### Testing Liquibase scripts
The Liquibase scripts can be tested locally by spinning up the generic database (db) and the liquibase container.

```
% docker-compose up -d db
% docker-compose up liquibase
```

The local file system is mounted into the liquibase container. This allows you to change the liquibase and shell scripts and run the changes by just re-launching the liquibase container. Note that all standard Liquibase caveats apply.

The PostGIS database will be available on your localhost's port $DB_PORT, allowing for visual inspection of the results.

### Other Helpful commands include:
* __docker-compose up__ to create and start the containers
* __docker-compose ps__ to list the containers
* __docker-compose stop__ or __docker-compose kill__ to stop the containers
* __docker-compose start__ to start the containers
* __docker-compose rm__ to remove all containers
* __docker network ls__ to get a list of local docker network names
* __docker network inspect XXX__ to get the ip addresses of the running containers
* __docker-compose ps -q__ to get the Docker Compose container ids
* __docker ps -a__ to list all the Docker containers
* __docker rm <containerId>__ to remove a container
* __docker rmi <imageId>__ to remove an image
* __docker logs <containerID>__ to view the Docker Compose logs in a container
