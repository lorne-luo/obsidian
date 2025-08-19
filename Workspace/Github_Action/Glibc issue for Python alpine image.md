To get python 3.12 environment in GHA selfhosed runner, devops team created **ghcr.io/selfwealth/swpython312:main** container for me.

But it still have issue:

* This container has no secret manager access. We can create a dedicated job to get the db credentials, but it requires an extra manual approvals
* This container is based `Alpine` linux, which uses `musl libc `, while one of my python dependency package awslambdaric require the headers file of `glibc `. I tried several way to install `glibc` on alpine, but not works.
