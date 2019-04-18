# CLI

The CLI (command line interface) is terminal application for Windows, Linux and OS X.

You can download the CLI at Github: https://github.com/Squidex/squidex-samples/releases

The CLI has two main advantages:

1. It is easy to automate things in your build and release processes, for example you can trigger nightly updates, schema migrations from one app to another or export content.
2. It is easier to integrate complex features, such as export to CSV, because it takes more time to write a good user interface than the export routine itself.

## How to use the CLI?

Hopefully the CLI itself is good enough so that you can use the integrated help to navigate through all the features.

The general structure of each command is

```
.\sq.exe [FEATURE] [COMMAND] [ARGS] [OPTIONAL_PARAMETERS]
````

Depending on your use cases you need a client in the **Developer** or even **Owner** role.

## How to manage configurations

The CLI can manage multiple configurations, so that you do not have to define the app, client and secret for each command.

1. Add a configuration

```
.\sq.exe config add [APP_NAME] [CLIENT_ID] [CLIENT_SECRET]
```

2. Show all configurations

```
.\sq.exe config list
```

or as table

```
.\sq.exe config list -t
.\sq.exe config list --table
```

3. Switch to another config

```
.\sq.exe config use [CONFIG_NAME]
```

## Use Case: How to sync schemas

> You need **Developer** role for this use case.

1. Go to first app and save the schema to a file

```
.\sq.exe config use app1
.\sq.exe config schemas get schema1 > schema.json
```

2. Go to second app and sync the schema from the saved file

```
.\sq.exe config use app2
.\sq.exe config schemas sync schema.json
```

3. OR: Sync it to another schema name

```
.\sq.exe config schemas sync schema.json --name other-schema
```

## Use Case: How to start a backup

> You need **Owner** role for this use case.

```
.\sq.exe backup create backup.zip
```

## Use Case: Export content

* Export content to CSV

```
.\sq.exe content export features --fields=id,version
```

* Export content to JSON

```
.\sq.exe content export features --format=JSON
```