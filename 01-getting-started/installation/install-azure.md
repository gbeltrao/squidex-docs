# Install on Azure

Please note that azure also supports Docker compose files so you can also follow the Docker tutorial, especially if it is important for you to be independent from your cloud provider.

In this tutorial I will also not teach you the basics of Azure. it is a very complicated product with thousands of features and you should be familiar with the basics before you follow this tutorial or just learn it on the fly.

## Requirements

Before you start you have to setup a few things first:

1. A resource group for all your squidex resources.
2. A service plan to host squidex.
3. A CosmosDB instance for your database.
4. A storage account for your assets.

## 1. Create the web app

Just create a new web app and follow the settings from the screenshot:

![Create App Service](../../images/started/azure/create-app-service.png)

## 2. Enable logging

In the next step we enable logging. This makes diagnostics easier.

Go to your app service and scroll down to menu item `Diagnostics log` and turn on file logging. You can then use the `log stream` to view all log entries.

![Enable logging](../../images/started/azure/logging.png)

## 3. Configure your application

Go to the `Configuration section` and choose `Application settings` to configure squidex.

> **IMPORANT**: After you change your configuration values you have to restart youir container. In our case the only option was to stop the app service and then start it again. The restart button did not work. Please write a comment if you know a better solution.

![All configuration values](../../images/started/azure/configuration.png)

Sensitive values are hidden, but configuration values for external authentication providers are empty to turn them off.

### 3.1. How to configure CosmosDB

Go to your CosmosDB instance to get the configuration settings:

![Cosmos DB settings](../../images/started/azure/cosmos.png)

### 3.2. How to configure Asset Store

Go to your storage account instance to get the configuration settings:

![Asset Store settings](../../images/started/azure/storage.png)

### 3.3. All settings

All basic settings:

```json
[
  {
    "name": "ASSETSTORE__AZUREBLOB__CONNECTIONSTRING",
    "value": "[ADD YOUR VALUE HERE]",
    "slotSetting": false
  },
  {
    "name": "ASSETSTORE__TYPE",
    "value": "AzureBlob",
    "slotSetting": false
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_URL",
    "value": "https://index.docker.io",
    "slotSetting": false
  },
  {
    "name": "EVENTSTORE__COSMOSDB__CONFIGURATION",
    "value": "[ADD YOUR VALUE HERE]",
    "slotSetting": false
  },
  {
    "name": "EVENTSTORE__COSMOSDB__MASTERKEY",
    "value": "[ADD YOUR VALUE HERE]",
    "slotSetting": false
  },
  {
    "name": "EVENTSTORE__TYPE",
    "value": "CosmosDB",
    "slotSetting": false
  },
  {
    "name": "IDENTITY__ADMINEMAIL",
    "value": "[ADD YOUR VALUE HERE]",
    "slotSetting": false
  },
  {
    "name": "IDENTITY__ADMINPASSWORD",
    "value": "[ADD YOUR VALUE HERE]",
    "slotSetting": false
  },
  {
    "name": "IDENTITY__GITHUBCLIENT",
    "value": "",
    "slotSetting": false
  },
  {
    "name": "IDENTITY__GOOGLECLIENT",
    "value": "",
    "slotSetting": false
  },
  {
    "name": "IDENTITY__MICROSOFTCLIENT",
    "value": "",
    "slotSetting": false
  },
  {
    "name": "STORE__MONGODB__CONFIGURATION",
    "value": "[ADD YOUR VALUE HERE]",
    "slotSetting": false
  },
  {
    "name": "STORE__MONGODB__ISCOSMOSDB",
    "value": "true",
    "slotSetting": false
  },
  {
    "name": "URLS__BASEURL",
    "value": "[ADD YOUR VALUE HERE]",
    "slotSetting": false
  },
  {
    "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
    "value": "false",
    "slotSetting": false
  }
]
```

### More issues? 

It is very likely a configuration problem and not related to hosting under azure. Go to the [Configuration](configuration.md) page.