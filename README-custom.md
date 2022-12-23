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
### Error: BATS_TMPDIR (/tmp) is not writable🚨 Error: The command exited with status 1
#### `faccessat2` sys call returning a `EPERM` error code instead of `ENOSYS` error code.

More specifically, `Linux 5.8` (release 5.8.18) released on 02 August 2020, introduced `faccessat2` sys call, which was then implemented in `glibc` (GNU C Library) (glibc 2.33), which returned different error codes: `EPERM` (139 - The operation is not permitted.), instead of `ENOSYS` (134 - The function is not implemented.).

`libseccomp` and `runc` which use `glibc` were affected and hence, this affected `moby` and therefore, `Docker`.

Paraphrased from Docker page: 
>`seccomp` (Secure Computing mode) is a linux kernel feature, which is used to restrict actions within the container. The seccomp() system call operates on the seccomp state of the calling process, to restrict your application's access. Default seccomp profile disables around 44 system calls out of 300+. 

Just a side note, upon further investigation it seems like the restrictive seccomp filter was causing access problems which then led to returning `EPERM` instead of `ENOSYS`. 

Ultimately, in our use case, this affected us in accessing the default `BATS_TMPDIR` directory: `/tmp` (`Error: BATS_TMPDIR (/tmp) is not writable🚨 Error: The command exited with status 1`), as we are doing `dind (docker in docker)`, when using the `buildkite-plugins/docker-buildkite-plugin` to do containerised builds with the `buildkite-plugins/buildkite-plugin-tester` image. 

References:
- Error Code Guide: https://www.ibm.com/docs/en/zos/2.5.0?topic=codes-return-errnos
- Red Hat Bug Report: https://bugzilla.redhat.com/show_bug.cgi?id=1900021
- Initial faccessat2 change for glibc: https://sourceware.org/git/?p=glibc.git;a=commitdiff;h=1cfb4715288845ebc55ad664421b48b32de9599c
- Follow up faccessat2 change for glibc: https://sourceware.org/git/?p=glibc.git;a=commitdiff;h=3d3ab573a5f3071992cbc4f57d50d1d29d55bde2
- Arch Linux Bug Report: https://bugs.archlinux.org/index.php?do=details&task_id=69563
- PR raised for faccessat2 issue: https://github.com/opencontainers/runc/pull/2750
- Issue raised for faccessat2 issue: https://github.com/opencontainers/runc/issues/2151
- IMPORTANT READ - BATS-core issue raised for /tmp not writable: https://github.com/bats-core/bats-core/issues/564
- Docker page for seccomp: https://docs.docker.com/engine/security/seccomp/
- Default Docker seccomp profile: https://github.com/moby/moby/blob/master/profiles/seccomp/default.json
- bats/bats docker hub image - tagged with latest-no-faccessat2 (v1.8.2): https://hub.docker.com/layers/bats/bats/latest-no-faccessat2/images/sha256-ab8b147d6fd604a25872580db473a61d446370af00addfe0a0d8b4394eae0f6b?context=explore


