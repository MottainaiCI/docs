# Docker compose

With Docker Compose, you will be able to start a ready-to-go instance in 3 steps!

The Docker Compose stack is using Redis as a broker to dispatch messages to agent.

## Step 1 - Start docker (prerequisite)

Make sure your docker daemon is running locally:

```bash
$> systemctl is-active docker || sudo systemctl start docker
```

## Step 2 - Install Docker-compose (prerequisite)

Make sure you have docker-compose installed, if is not available as a package for your Linux distribution, [check the official docs](https://docs.docker.com/compose/install/), or you can:

```bash
$> sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$> sudo chmod +x /usr/local/bin/docker-compose
```

## Step 3 - Start the server

```bash

# Clone the repo
$> git clone https://github.com/MottainaiCI/mottainai-server

# The docker-compose file is under contrib/docker-compose
$> cd mottainai-server/contrib/docker-compose

# Start the docker-compose stack. Drop -d if you want to run it in the foreground
$> docker-compose up -d
```

When the initialization sequence is completed, you are good to go, you can browse [http://127.0.0.1:4545/](http://127.0.0.1:4545/) to see your instance.

Register an account (the first registered is automatically given admin rights) and [check on how to run an ephemeral agent](agent-ephemeral.md).