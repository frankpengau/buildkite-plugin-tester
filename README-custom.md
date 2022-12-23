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
docker push frankpengau/buildkite-plugin-tester:bats-latest-1-8-2
```

## Docker Logout
```
docker logout
```