boot2docker-mysql-init
===

An init script replacement that fixes permission issues on boot2docker mounted
volumes.

This script is meant to be a temporary solution until file syncing issues
are fixed.

background
---

There's a boot2docker/non-native-docker issue where the container fails to run.
This is whenever a data volume is mounted on `/var/lib/mysql`. This issue is
discussed here: https://github.com/docker-library/mysql/issues/99.

A solution was posted by [@motin](https://github.com/motin), quoting:

```code
The solution is to make sure that mysql runs using the same user and group ids
as your local OSX user.
```

installation/usage
---

there are two ways to install the fix.

You can download the script by running this command:

```sh
curl -L https://raw.githubusercontent.com/thejpanganiban/boot2docker-mysql-init/master/init.sh > init.sh
```

### build new image

Create a new dockerfile
```sh
cat <<EOT >> Dockerfile
FROM mysql:latest
ADD init.sh /tmp/init.sh
CMD ["/tmp/init.sh"]
EOT
```

### mount in compose

Update compose.yml

```yml
mysql:
  image: mysql:latest
  volumes:
    # ...
    - init.sh:/tmp/init.sh
  command: /tmp/init.sh
```

### attribution

This script was original written by [@motin](https://github.com/motin).
