# liberty-okteto-webapp

Okteto sample webapp on Open Liberty.

This repo holds a simple webapp and few samples showing how to speed up the inner development loop when working with containers with Open Liberty. There are samples for local and remote development.

Also notice that Open Liberty runtime itself will be automagically downloaded by Maven.

## Local development

To run/debug locally you have a few choices. All of them will:

- Expose a sample webapp in http://localhost:8080
- Expose the Java debug port in localhost:7777

### Liberty Dev Mode

Liberty dev mode hot reloads classes and configs and exposes a debug port, but you need a local JDK (of course):

```sh
mvn liberty:dev
```

The webapp will be available in http://localhost:8080 and Java debugging in 7777 port. It can be stopped with "^C" or gracefully with the command:

```sh
mvn liberty:stop
```

Note: We changed Liberty HTTP port from 9080 to 8080 for later use in Okteto.

### Docker-compose workflow

You can also use docker-compose for local development (no local dependency other than docker and docker-compose).
Project and maven cache folders are mounted in the container before starting Liberty in dev mode: 

```sh
docker-compose up
```

Both http and debug ports are bound to 8080 and 7777 ports.

## Remote development with Okteto

Remote development strategies simplify project onboarding and good practices. Some software projects rely on a complex infrastructure that cannot be ran locally, so deploying it in a remote cluster makes sense. On the other hand, a developer's inner loop should not rely on the lengthy "build/push/pull/run" workflow for container images.

[Okteto](https://okteto.com/) solves this by creating remote development environments in kubernetes tunneled to local ports.

To run/debug remotely you have a few choices. All of them will:

- Expose a sample webapp in http://localhost:8080
- Expose the Java debug port in localhost:7777

### Using Okteto (any IDE)

The `okteto.yml` file defines the remote development container behaviour and local tunnels.

With a simple command you can start a remote dev environment in Okteto:

```sh
okteto up
```

Once inside your okteto remote shell you can start Liberty in dev mode (hot reloading classes and configs):

```sh
mvn liberty:dev
```

The webapp can be found through the local tunnel in http://localhost:8080, as well as any IDE can attach itself to the debug port at 7777.

### Using Okteto with VS Code Remote

With a simple command you can start a remote dev environment in Okteto:

```sh
okteto up --remote 2222
```

Okteto creates an `liberty-okteto-webapp.okteto` host entry in `~/.ssh/config` that VS Code can use.
