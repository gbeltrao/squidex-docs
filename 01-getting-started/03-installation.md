# Installation

This guide will explain the installation process. Please contact us in Gitter or Slack if you have issues.

## 1. Create the deployment artifacts

Read [Building](02-building.md) to learn how to create the deployment artifacts.

## 2. Deploy to Docker

We provide a docker-compose configuration under: https://github.com/Squidex/squidex-docker/blob/master/standalone/docker-compose.yml

it will run 4 containers:

* MongoDB
* Squidex
* Nginx as Reverse Proxy with Https Termination
* Nginx Sidecar to provision certificates with LetsEncrypt.

Open the `.env` file and set all variables:

* `SQUIDEX_DOMAIN`: Your domain name
* `SQUIDEX_ADMINEMAIL`: The email address of the admin user.
* `SQUIDEX_ADMINPASSWORD`: The password of the admin user (Must contain lowercase, uppercase letter, number and special character.)

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

* `urls:baseUrl`: The base url under which Squide is running.
* `identity:adminEmail`: The email address of the admin user.
* `identity:adminPassword`: The password of the admin user (Must contain lowercase, uppercase letter, number and special character.)

Set `identity:googleClient`, `identity:githubClient` and `identity:microsoftClient` to empty to disable authentication with third party providers.

Read the comments of the `appsettings.json` file to understand all configuration options.