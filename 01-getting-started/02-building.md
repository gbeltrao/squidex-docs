# Building

## 1. Build for docker

We provide docker images on docker hub: https://hub.docker.com/r/squidex/squidex/

* `squidex/squidex:latest` is the latest stable version.
* `squidex/squidex:dev` is the current dev version (master branch).

To build a custom image use our multistage dockerfile. Just run:

    docker build . -t my/squidex:latest

## 2. Build for manual deployment

When you want to deploy to IIS or Nginx you have to build manually. 
You can then find the files under `$SQUIDEX/publish`. 

### 2.1. Build with docker

Run the following commands in Powershell or bash to build Squidex with docker:

    # Build the image
    docker build . -t squidex-build-image -f dockerfile.build

    # Open the image
    docker create --name squidex-build-container squidex-build-image

    #Copy the output to the host file system
    docker cp squidex-build-container:/out ./publish

    # Cleanup
    docker rm squidex-build-container

Under windows just use the `build.ps` script.

### 2.2. Build without docker

If you don't want to use docker, you can also build it manually. You have to execute the following commands:

    cd $SQUIDEX$/src/Squidex

    npm install
    npm rebuild node-sass --force
    npm run build:copy
    npm run build

    dotnet restore
    dotnet publish --configuration Release --output "../../publish"

We recommend to build Squidex with docker, because it ensures that you have a clean environment. Because of the docker [layers](http://bitjudo.com/blog/2014/03/13/building-efficient-dockerfiles-node-dot-js/) the build is not much slower and can be even faster in some situations.