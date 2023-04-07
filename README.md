# webENM docker container

## Requirements
- working docker server

## Installation

### MDB Connector Server

- clone the [mdb_connector_server](https://github.com/Sarius121/mdb_connector_server) into the *mdb_connector_server* directory
- rename the file *src/main/resources/application.properties-example* to *src/main/resources/application.properties*
- edit the *src/main/resources/application.properties* file:
    - set `dbfile.directory` to `/var/www/html/grade-files/tmp/`
    - set `mdbconnector.apisecret` to some randomly generated secret
    - set `mdbconnector.restricttolocalhost` to `false`
- edit the *pom.xml* file:
    - replace the `war` in the 'packaging' line by `jar`:
        ```
        ...
        <packaging>jar</packaging>
        ...
        ```

### WebENM Frontend

- clone the [schild_webenm](https://github.com/Sarius121/schild_webenm) into the *schild_webenm* directory
- rename the *constants.php-example* files to *constants.php*
- edit the *lib/MDBConnector/constants.php* file:
    - set `API_SECRET` to the same secret as in the MDB Connector Server configuration
    - set `MDB_CONN_PATH` to the empty string
    - set `MDB_CONN_SERVER` to `mdb-connector-server`
- edit the other configuration files as you need them (look at the schild_webenm-README for more information)

### deploy

You can now run the containers by typing
```
docker compose up
```
into a terminal.

Notice that you have to run
```
docker compose up --build
```
whenever you make changes in the configuration files.

## Known issues
- The docker command will fail if the line endings of the files are not LF. Make sure that they are LF (especially for the *mvnw*-file).