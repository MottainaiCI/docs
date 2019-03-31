# Ephemeral agents

Mottainai will guide you in the WebUI to create ephemeral agents to connect to your instance.

## Step 1 - Create a new Node

In the Web Interface, go over the Node section, and hit the *Create* button (this can be performed also over CLI).

You will see a new node in the table. Click on it

## Step 2 - Run the agent in the docker container

Go on the node page (click on the Node id in the webui, that we just created before).

You will see a tooltip which is giving you a docker command, you can copy-paste that command and run it in a machine with docker. That's it!

*Note*: If your instance is created with Docker-compose, you need to replace all the ```redis``` (host) occurrence that you see in the command with your remote machine IP (e.g. if you are in a LAN with ip 192.168.1.2, you have to replace ```redis://redis:6379/2``` with ```redis://192.168.1.2:6379/2``` manually)