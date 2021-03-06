# Zenko Quick Testing Swarm Stack

This docker service stack describes a simple Zenko setup for quick testing
with non-production data.

## Preparing

Swarm mode needs to be enabled on the local docker daemon. See
[this tutorial](https://docs.docker.com/engine/swarm/swarm-tutorial/)
for more information on Swarm mode.

## Deploying

Deploy the stack:

```
$ docker stack deploy -c docker-stack.yml zenko-testing
ID            NAME              MODE        REPLICAS  IMAGE
5s5ny9y859sj  zenko-testing_lb  replicated  1/1       nginx:alpine
ei95xqodynoc  zenko-testing_s3  replicated  1/1       scality/s3server:latest
```

Check that the services are up:

```
$ docker stack services zenko-testing
ID            NAME              MODE        REPLICAS  IMAGE
5s5ny9y859sj  zenko-testing_lb  replicated  1/1       nginx:alpine
ei95xqodynoc  zenko-testing_s3  replicated  1/1       scality/s3server:latest
```

## Testing

Using [awscli](https://aws.amazon.com/cli/), we can perform S3 operations
on our Zenko stack:
 > IMPORTANT: when using default port 80, it should never be specified after the
 > endpoint address. If using a custom port, it must be specified.
```
$ export AWS_ACCESS_KEY_ID=accessKey1
$ export AWS_SECRET_ACCESS_KEY=verySecretKey1
$ aws s3 --endpoint http://localhost mb s3://bucket1 --region=us-east-1
make_bucket: bucket1
$ aws s3 --endpoint http://localhost ls
2017-06-15 16:42:58 bucket1
$ aws s3 --endpoint http://localhost cp README.md s3://bucket1
upload: ./README.md to s3://bucket1/README.md
$ aws s3 --endpoint http://localhost ls s3://bucket1
2017-06-15 17:36:10       1484 README.md
```
