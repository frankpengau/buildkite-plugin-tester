## Docker Login
```
docker login
```

## Docker Image Build
```
docker-compose -f docker-compose-custom.yml run --rm tag
```

## List images
```
docker image ls
```

## Run image
```
docker-compose -f docker-compose-custom.yml run --rm tests
```

## Push Image to Docker Hub
```
docker push frankpengau/buildkite-plugin-tester:bats-latest-no-faccessat2
```

## Docker Logout
```
docker logout
```

# NOTES
## Issues
### `faccessat2` sys call returning a `EPERM` error code instead of `ENOSYS` error code.

More specifically, `Linux 5.8` (release 5.8.18) released on 02 August 2020, introduced `faccessat2` sys call, which was then implemented in `glibc` (GNU C Library) (glibc 2.33), which returned different error codes: `EPERM` (139 - The operation is not permitted.), instead of `ENOSYS` (134 - The function is not implemented.).

`libseccomp` and `runc` which use `glibc` were affected and hence, this affected `moby` and therefore, `Docker`.

Ultimately, in our use case, this affected us in accessing the default `BATS_TMPDIR` directory: `/tmp`, as we are doing `dind (docker in docker)`, when using the `buildkite-plugins/docker-buildkite-plugin` to do containerised builds with the `buildkite-plugins/buildkite-plugin-tester` image. 