# Gerrit code review along with Jenkins CI service

## Initialize and start the service

### Step-1: Add YOUR config

Add config for SMTP server in [gerrit.config](./gerrit/etc/gerrit.config) at `[sendemail]` section, and the password for your configed email in [secure.config](./gerrit/etc/secure.config) at `[sendemail]` section.

You can change the server name (`review.test.netease.com`) in [docker-compose.yaml](./docker-compose.yaml) and [nginx.conf](./nginx/nginx.conf) and [gerrit.config](./gerrit/etc/gerrit.config) to your custom server name(`your-configed-server-name`).

### Step-2: Initialize Gerrit DB and Git repositories

```shell
docker-compose up -d postgres
```

Uncomment in [docker-compose.yaml](./docker-compose.yaml) the Gerrit init step entrypoint and run Gerrit with docker-compose in foreground.

```shell
docker-compose up gerrit
```

Wait until you see in the output the message `Initialized /var/gerrit` and then the container will exit.

**Note: Maybe you need to change the owner of directory `gerrit` to the user used by the gerrit image in some system (Debian for example), use `docker run --rm $image_name id $user` to get the uid and gid.**

See [Docker Gerrit Guide](https://gerrit.googlesource.com/docker-gerrit/#initialize-gerrit-db-and-git-repositories-with-docker) for complete instructions.

### Step-3: Start the service

Comment out the gerrit init entrypoint in [docker-compose.yaml](./docker-compose.yaml) and start all the docker-compose nodes:

```shell
docker-compose up -d
```

### Step-4: Access Gerrit and Jenkins

You can access Gerrit at `http://your-configed-server-name/gerrit` (or `http://your-configed-server-name/`) and Jenkins in `http://your-configed-server-name/jenkins`.

## Config Gerrit and Jenkins

See [Gerrit Trigger](https://wiki.jenkins.io/display/JENKINS/Gerrit+Trigger).