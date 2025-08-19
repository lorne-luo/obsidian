# Issue

We run a atmoz/sftp container as a dependency service for integration test, it's tricky to set up user and ssh key for this container.

When launching dependency service like sftp in GHA, we can't do volume bin like 

```
- ./sftp_key/id_rsa.pub:/home/bgl/.ssh/keys/id_rsa.pub:ro`
```

When service is launching, the code base is not been checked out yet, we can't map any local file into container when it's starting.

- docker 's volumes binding will not raise any error, if host file/folder not exist, it will create a empty folder
- even we can run `docker cp` to copy the `id_rsa` in following step, it still wont work, because the folder `/home/bgl/.ssh/keys/` will only be loaded when container launching


# Solution

Use `/home/bgl/.ssh/authorized_keys`

```yaml

  sonarcloud:
    name: Tests and SonarCloud
    runs-on: ubuntu-22.04
    services:
      sftp:
        image: atmoz/sftp
        env:
          SFTP_USERS: "bgl:bgl:::incoming class:class:::incoming xplan:xplan:::incoming"
        ports:
          - 2222:22
        volumes:
          # can not bind directly to dockerfiles/sftp_key/id_rsa.pub as the repo is not checkout yet when container is raising
          # here we bind to a temporary directory and copy the id_rsa.pub later
          # and this only works with /home/bgl/.ssh/authorized_keys, not applicable for /home/bgl/.ssh/keys/id_rsa.pub
          - /tmp/ssh_key:/home/bgl/.ssh/
```

```
# in following step, do
    # Copy SFTP user config and SSH keys for SFTP
    sudo cp dockerfiles/sftp_key/id_rsa.pub /tmp/ssh_key/authorized_keys
```
