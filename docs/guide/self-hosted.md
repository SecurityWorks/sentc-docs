# Self-hosting

The sdk connects to an api. 
The api will store the encrypted user and group keys and will also manage the key rotation and group member.

## Docker

You can choose docker to run the api. To get started, download the [sentc/hosting](https://github.com/sentclose/hosting) repo.
It contains basic docker-compose to get the server running.

```bash:no-line-numbers
git clone https://github.com/sentclose/hosting
cd hosting
```

Next copy and rename `.env.sample` to `.env` and `sentc.env.sample` to `sentc.env`

The `.env` file contains config about the used container. You can change the version of each container.

Optional but recommended: Change the mysql env too.

Next you have to create a root key with the `sentc key gen tool` and paste it into `ROOT_KEY` in the `sentc.env` file.

#### Root key generation

You can use the docker image to generate a key:

```bash:no-line-numbers
docker-compose -f key_gen/docker-compose.yml up
```

Without the -d flag to get the key output. Copy your key, and then you can down the container:

```bash:no-line-numbers
docker-compose -f key_gen/docker-compose.yml down -v
```

And delete the image

```bash:no-line-numbers
docker image rm sentc/key_gen
```

### Start

The default is mariadb with redis server.

```bash:no-line-numbers
docker-compose -f mysql/docker-compose.yml up -d
```

Now everything is running and you can start.

### Non default

If you are using an external Database, or a Database which is running native, then use the `mysql/docker-compose.stand_alone.yml` file.

Before starting set also the both Env: `MYSQL_HOST` (your host where the db is running), `MYSQL_DB` (the database name).

```bash:no-line-numbers
docker-compose -f mysql/docker-compose.external_db.yml up -d
```

Keep in mind that this will use the array cache as default not redis. If you have redis also running, set the Env `CACHE` to 2
and the Env `REDIS_URL` to your running redis url instance.