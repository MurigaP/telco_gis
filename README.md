# Telco_Service

GeoNode template project. Generates a django project with GeoNode support.

## Table of Contents

-  [Quick Docker Start](#quick-docker-start)
-  [Developer Workshop](#developer-workshop)
-  [Create a custom project](#create-a-custom-project)
-  [Start your server using Docker](#start-your-server-using-docker)
-  [Run the instance in development mode](#run-the-instance-in-development-mode)
-  [Run the instance on a public site](#run-the-instance-on-a-public-site)
-  [Stop the Docker Images](#stop-the-docker-images)
-  [Backup and Restore from Docker Images](#backup-and-restore-the-docker-images)
-  [Recommended: Track your changes](#recommended-track-your-changes)
-  [Hints: Configuring `requirements.txt`](#hints-configuring-requirementstxt)

## Quick Docker Start

  ```bash
    python3.10 -m venv ~/.venvs/telco_service
    source ~/.venvs/telco_service/bin/activate

    pip install Django==4.2.9

    mkdir ~/telco_service
    cd ~/telco_service
    python create-envfile.py 
  ```
`create-envfile.py` accepts the following arguments:

```bash
  docker compose build
  docker compose up -d
```


  ```

## Create a custom project

1. Create the .env file

    An `.env` file is requird to run the application. It can be created from the `.env.sample` either manually or with the create-envfile.py script.

    The script accepts several parameters to create the file, in detail:

    - *hostname*: e.g. master.demo.geonode.org, default localhost
    - *https*: (boolean), default value is False
    - *email*: Admin email (this is required if https is set to True since a valid email is required by Letsencrypt certbot)
    - *env_type*: `prod`, `test` or `dev`. It will set the `DEBUG` variable to `False` (`prod`, `test`) or `True` (`dev`)
    - *geonodepwd*: GeoNode admin password (required inside the .env)
    - *geoserverpwd*: Geoserver admin password (required inside the .env)
    - *pgpwd*: PostgreSQL password (required inside the .env)
    - *dbpwd*: GeoNode DB user password (required inside the .env)
    - *geodbpwd*: Geodatabase user password (required inside the .env)
    - *clientid*: Oauth2 client id (required inside the .env)
    - *clientsecret*: Oauth2 client secret (required inside the .env)
    - *secret key*: Django secret key (required inside the .env)
    - *sample_file*: absolute path to a env_sample file used to create the env_file. If not provided, the one inside the GeoNode project is used.
    - *file*: absolute path to a json file that contains all the above configuration

     **NOTE:**
    - if the same configuration is passed in the json file and as an argument, the CLI one will overwrite the one in the JSON file
    - If some value is not provided, a random string is used

      Example USAGE

      ```bash
      python create-envfile.py -f /opt/core/geonode-project/file.json \
        --hostname localhost \
        --https \
        --email random@email.com \
        --geonodepwd gn_password \
        --geoserverpwd gs_password \
        --pgpwd pg_password \
        --dbpwd db_password \
        --geodbpwd _db_password \
        --clientid 12345 \
        --clientsecret abc123 
      ```

      Example JSON expected:

      ```JSON
      {
        "hostname": "value",
        "https": "value",
        "email": "value",
        "geonodepwd": "value",
        "geoserverpwd": "value",
        "pgpwd": "value",
        "dbpwd": "value",
        "geodbpwd": "value",
        "clientid": "value",
        "clientsecret": "value"
      } 
      ```

### Start your server
*Skip this part if you want to run the project using Docker instead* see [Start your server using Docker](#start-your-server-using-docker)


### Start your server using Docker

You need Docker 1.12 or higher, get the latest stable official release for your platform.
Once you have the project configured run the following command from the root folder of the project.

1. Run `docker-compose` to start it up (get a cup of coffee or tea while you wait)

    ```bash
    docker-compose build --no-cache
    docker-compose up -d
    ```

    ```bash
    set COMPOSE_CONVERT_WINDOWS_PATHS=1
    ```

    before running `docker-compose up`

2. Access the site on http://localhost/

## Run the instance in development mode

### Use dedicated docker-compose files while developing

**NOTE**: In this example we are going to keep localhost as the target IP for GeoNode

  ```bash
  docker-compose -f docker-compose.development.yml -f docker-compose.development.override.yml up
  ```


### Startup the image

```bash
docker-compose up --build -d
```

### Stop the Docker Images

```bash
docker-compose stop
```

### Fully Wipe-out the Docker Images

**WARNING**: This will wipe out all the repositories created until now.

**NOTE**: The images must be stopped first

```bash
docker system prune -a
```

## Backup and Restore from Docker Images

### Run a Backup

```bash
SOURCE_URL=$SOURCE_URL TARGET_URL=$TARGET_URL ./telco_service/br/backup.sh $BKP_FOLDER_NAME
```

- BKP_FOLDER_NAME:
  Default value = backup_restore
  Shared Backup Folder name.
  The scripts assume it is located on "root" e.g.: /$BKP_FOLDER_NAME/

- SOURCE_URL:
  Source Server URL, the one generating the "backup" file.

- TARGET_URL:
  Target Server URL, the one which must be synched.


## Recommended: Track your changes

Step 1. Install Git (for Linux, Mac or Windows).

Step 2. Init git locally and do the first commit:

```bash
git init
git add *
git commit -m "Initial Commit"
```

Test geonode on [http://localhost:8888/](http://localhost:8888/)

