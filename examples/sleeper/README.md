# Sleeper Function Image

This images compares the payload info with the header.

## Requirements

- IronFunctions API

## Development

### Building image locally

```
# SET BELOW TO YOUR DOCKER HUB USERNAME
USERNAME=YOUR_DOCKER_HUB_USERNAME

# build it
./build.sh
```

### Publishing to DockerHub

```
# tagging
docker run --rm -v "$PWD":/app treeder/bump patch
docker tag $USERNAME/func-sleeper:latest $USERNAME/func-sleeper:`cat VERSION`

# pushing to docker hub
docker push $USERNAME/func-sleeper
```

### Testing image

```
./test.sh
```

## Running it on IronFunctions

### Let's define some environment variables

```
# Set your Function server address
# Eg. 127.0.0.1:8080
FUNCAPI=YOUR_FUNCTIONS_ADDRESS
```

### Running with IronFunctions

With this command we are going to create an application with name `sleeper`.

```
curl -X POST --data '{
    "app": {
        "name": "sleeper",
    }
}' http://$FUNCAPI/v1/apps
```

Now, we can create our route

```
curl -X POST --data '{
    "route": {
        "image": "'$USERNAME'/func-sleeper",
        "path": "/sleeper",
    }
}' http://$FUNCAPI/v1/apps/sleeper/routes
```

#### Testing function

Now that we created our IronFunction route, let's test our new route

```
curl -X POST --data '{"sleep": 5}' http://$FUNCAPI/r/sleeper/sleeper
```
