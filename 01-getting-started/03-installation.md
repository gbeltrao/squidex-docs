# Installation

This guide will explain the installation process. Please contact us in Gitter or Slack if you have issues.

## 1. Create the deployment artifacts

Read the [Building](02-building.md) to learn how to create the deployment artifacts.

## 2. Deploy to Docker

We provide a docker-compose configuration under: https://github.com/Squidex/squidex-docker/blob/master/standalone/docker-compose.yml

It will run 4 containers:

* MongoDB
* Squidex
* Nginx as Reverse Proxy with Https Termination
* Nginx Sidecar to provision certificates with LetsEncrypt.

### Steps

#### 1. Edit Settings

Open the `.env` file and set all variables:

* `SQUIDEX_PROTOCOL`: Keep it unchanged. You can set it to http to disable secure connections.
* `SQUIDEX_FORCE_HTTPS`: Keep it unchanged. You can set it to false to disable permanent redirects from http to https.
* `SQUIDEX_DOMAIN`: Your domain name, e.g. we use `cloud.squidex.io`
* `SQUIDEX_ADMINEMAIL`: The email address of the admin user.
* `SQUIDEX_ADMINPASSWORD`: The password of the admin user (Must contain a lowercase and uppercase letter, a number and a special character.)

You can keep the other settings empty for now.

#### 2. Create the MongoDB database folder

The data will be stored outside of the docker container to simplify the backups. Create the folder with

    sudo mkdir /var/mongo/db

#### 3. Run the docker-compose file

    docker-compose up -d

### Troubleshooting

#### I can login but requests fail:

The ASP.NET Core must make requests to the container itself. We host identity-server and the api in the same application and the api has to make a request to identity-server to read the configuration and to verify tokens. In some environments we had to open the ports in the firewall:

     firewall-cmd --zone=public --add-port=443/tcp

## 3. Deploy to IIS

Please follow this tutorial to learn how deploy an ASP.NET Core application to IIS:

 [https://docs.microsoft.com/en-us/aspnet/core/publishing/iis](https://docs.microsoft.com/en-us/aspnet/core/publishing/iis).

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

## 4. Basic configuration

We use the [ASP.NET Core Configuration](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration) model for all settings. You can configure Squidex with environment variables, command line arguments and the `appsettings.json` file. Using a mix of all these options can be very helpful.

These are the most important settings:

* `urls:baseUrl`: The base url under which Squidex is running. It is used to generate hyperlinks and to make redirects with the correct host name. In some environments, squidex is running behind several proxies, e.g. cloudflare, google load balancer and so on. In these cases the original host name might get lost. Therefore we introduced this configuration value.
* `identity:adminEmail`: The email address of the admin user.
* `identity:adminPassword`: The password of the admin user (Must contain lowercase, uppercase letter, number and special character.)

Set `identity:googleClient`, `identity:githubClient` and `identity:microsoftClient` to empty to disable authentication with third party providers.

Read the comments of the `appsettings.json` file to understand all configuration options.

### Troubleshooting

#### Login fails with 'Operation failed' message

Please check the logs to see detailed error messages. Typically the login fails, because the `urls:baseUrl` setting has an invalid value. When you login, the Management UI tells the identity system where to redirect the user to. The frontent (running on the client) knows the current url and might use `https://squidex.local/client-callback-popup.html`. THe identity system uses the `url:baseUrl` setting to calculate the allowed redirect urls (which is set to `http://localhost:5000` by default). When it has a wrong configuration, the identity system cannot verify the redirect url and rejects the login attempt. Therefore it is very important to ensure that the `urls:baseUrl` config has the right value.