# Developing

You can find the source code on github: [https://github.com/squidex/squidex](https://github.com/squidex/squidex)

## Tools
To work with the source code you need the following tools:

* [Visual Studio Code](https://code.visualstudio.com/) or [Visual Studio 2017](https://www.visualstudio.com/vs/visual-studio-2017-rc/)
* [Node.js](https://nodejs.org/en/)
* [.NET Core SDK](https://www.microsoft.com/net/download/core#/current) (Already part of Visual Studio 2017)
* [MongoDB](https://www.mongodb.com/)
* Optionally: [Redis](https://redis.io/download)

We also provide ready to use docker configurations: [https://github.com/squidex/squidex-docker](https://github.com/squidex/squidex-docker). Just execute the following commands to get a mongodb and redis installation for development:

1. `git clone https://github.com/squidex/squidex-docker`
2. `cd squidex-docker/dependencies`
3. `docker-compose up -d`

> Please Note: MongoDB and Redis are not password protected. Do not expose it to the internet.

## How to run the Squidex

Squidex has two parts:

1. The frontend, written in [Angular](https://angular.io) and using [Webpack2](https://webpack.js.org/)
2. The backend, written in [ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/)

You have to run both indendently. It might be a little bit annoying but it reduces the startup time, especially when you want to rerun the backend after you made some changes. Open two terminals and run the following commands to start the frontend and the backend.

### How to run the Frontend?

1. `cd src/Squidex/Squidex` (Go to the web application project)
2. `npm i` (Install all dependencies for the frontend)
3. `npm run dev` (Runs the webpack dev server)
4. Optionally: `npm test` (Runs the unit tests and listens for changes)
5. Optionally: `npm run test:coverage` (Runs the unit tests and calculates the test coverage).

Once the dev server is running, it listens to file changes and recompiles the app automatically. Have an eye to the terminal, because it also shows error, when it cannot compile the typescript code.

### How to run the Backend?

1. `cd src/Squidex/Squidex` (Go to the web application project)
2. `dotnet restore` (Install all dependencies)
3. `dotnet run` (Run the API)

We use webpack for the frontend code. To run the application locally you have to run the api and 