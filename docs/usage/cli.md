# Command Line Interface

You can use the command line interface to create, track and manage your cluster.
```
mottainai-cli --help                                      
Mottainai CLI
Copyright (c) 2017-2018 Mottainai

Command line interface for Mottainai clusters

Usage:
   [flags]
   [command]

Examples:
$> mottainai-cli -m http://127.0.0.1:8080 task create --json task.json

$> mottainai-cli -m http://127.0.0.1:8080 namespace list


Available Commands:
  help        Help about any command
  namespace   Manage namespaces
  node        Manage nodes
  pipeline    Manage Pipeline of Task
  plan        Manage Planning of Task
  profile     Manage CLI Profiles
  simulate    Simulate Agent Command
  storage     Manage storages
  task        Manage tasks
  token       Manage tokens
  user        Manage users

Flags:
  -k, --apikey string    Mottainai API key (default "fb4h3bhgv4421355")
  -h, --help             help for this command
  -m, --master string    MottainaiCI webUI URL (default "http://localhost:8080")
  -p, --profile string   Use specific profile for call API.
      --version          version for this command

Use " [command] --help" for more information about a command.
```


## Tasks

After [defining a task](tasksandpipelines.md) you can use the CLI to create tasks and pipelines to be executed remotely
by the nodes belonging to your cluster.
