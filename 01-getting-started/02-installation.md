# Installation

This guide will explain the installation process. Please contact us in Gitter or Slack if you have issues.

For this guide we say that `$SQUIDEX$` is the path to your local copy of the source code.

## 1. Make a build

There are several ways to make a build.

### Using Docker

The simplest approach is to run the build with docker. For windows there is a build script at `$SQUIDEX$/build.ps1`, but you can also just copy the commands and run them by yourself if you are working with Linux or MacOS.

The build script will copy all files for the release to `$SQUIDEX$/publish`. It also contains a docker file, so you can just run the following command to build a docker image:

    docker build $SQUIDEX$/publish -t my/squidex:latest

### Build it manually

If you don't want to use docker, you can also build it manually. You have to execute the following commands:

    cd $SQUIDEX$/src/Squidex

    npm install
    npm run build:copy
    npm run build

    dotnet restore
    dotnet publish --configuration Release --output "../../publish"

You can find the files under `$SQUIDEX/publish`. We recommend to build Squidex with docker, because it ensures that you have a clean environment. Because of the docker [layers](http://bitjudo.com/blog/2014/03/13/building-efficient-dockerfiles-node-dot-js/) the build is not much slower and can be even faster in some situations.

## 2. Deployment

If you are using docker, just deploy your container, e.g. with kubernetes or docker swarm.

Hosting on IIS can be more tricky sometimes, but you can follow this tutorial: [https://docs.microsoft.com/en-us/aspnet/core/publishing/iis](https://docs.microsoft.com/en-us/aspnet/core/publishing/iis).

### Troubleshooting

We had some problems with ASP.NET Core under IIS. Perhaps the following two tipps might help you:

#### HTTP Error 502.5 - Process Failure

It is very important that you restart IIS after you have installed .NET Core Windows Server Hosting. Restart the server or execute net stop was /y followed by net start w3svc from a command prompt with elevated permissions to pick up a change to the system PATH.

#### HTTP Error 504 - Method not allowed

This can happen when you try to make an API call with the PUT or DELETE Verb. For example when you use the Management UI. The reason is that WebDAV might be installed on your server and it blocks these verbs. You have to add the following lines to the `Web.config` file.

    <system.webServer>
        <modules runAllManagedModulesForAllRequests="false">
            <remove name="WebDAVModule" />
        </modules>
    </system.webServer>

## 3. Basic Configuration

We use the [ASP.NET Core Configuration](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration) model for all settings. You can configure Squidex wit environment variables, command line arguments and the `appsettings.json` file. Using a mix of all these options can be very helpful.

These are the most important settings.

### Authentication

At the moment Squidex only supports Google authentication (but we are working on more authentication options). Therefore you have to create a google project first and enable authentication. Please follow this tutorial [https://docs.microsoft.com/en-us/aspnet/core/security/authentication/social/google-logins)](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/social/google-logins).

When you are asked to enter the **Authorized redirect URIs** please ensure that you add the following url: `https://<YOUR-DOMAIN>/identity-server/signin-google` or `http://<YOUR-DOMAIN>/identity-server/signin-google` if you don't want to use https.

### Base Url 

We also need to setup the base url so that the API and the Management UI can create Urls correctly. Change the base url under `squidex:urls:baseUrl`. It is very important that it has the correct host name and protocol.

### Https 

It is highly recommended to use https and ssl for security reasons. You don't have to buy a certificate, there are some free options:

1. You can use certificates from [https://letsencrypt.org/](https://letsencrypt.org/). Please note that these certificates are also valid for 60 days. Therefore you need a sidekick application for your web server that automatically installs and updates the credentials.

    * For Nginx and Apache there is [https://certbot.eff.org/](https://certbot.eff.org/).
    * For IIS under Windows you can use [https://github.com/Lone-Coder/letsencrypt-win-simple](https://github.com/Lone-Coder/letsencrypt-win-simple). It is a simple console application that will guide you through the process. Futhermore it installs a windows service that handles the update of the certificate. It is very likely that you will get an error, if you try it out for the first time. Please read the following documentation what to do in this case: [https://github.com/Lone-Coder/letsencrypt-win-simple/wiki/web.config](https://github.com/Lone-Coder/letsencrypt-win-simple/wiki/web.config).

2. You can use [https://www.cloudflare.com/](https://www.cloudflare.com/) in front of your Squidex installation. It will act as a reverse proxy and handle all incoming traffic. Usually it also caches the output, but there are settings to disable it. But you also need a certificate on your web server if you want to have end to end encryption.

To enforce https you can also set the `squidex:urls:enforeHttps` configuration value to `true`.

### Running in a cluster

If you want to run Squidex in a cluster, there are two settings that are interesting:

1. `squidex:clusterer:type` defines the clusterer. It is basically a set of classes that provide everything you need to run Squidex on multiple hosts. At the moment you can set it to `None` and `Redis`. Redis is needed to establish a communication channel between the hosts using PubSub and to store the encryption keys (Read more about [Data Protection](https://docs.microsoft.com/en-us/aspnet/core/security/data-protection/) in ASP.NET Core).

2. `squidex:handleEvents` defines if the Squidex instance will handle the CQRS events to populate the read stores. It is very important that **exactly one** host handles the events to guarantee that the events are processed in the right order. We recommend that you set this setting to false in the `appsettings.json` file and to overwrite it with an environment variable on one of the machines. If you are using kubernetes you can create a deployment with one instance for your event handler. 
