# Command Line Interface

You can use the command line interface to create, track and manage your cluster.
```
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
  setting     Manage Infrastructure settings
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

Use " [command] --help" for more information about a command.

```
## Authentication

**Note:** The CLI options **--master** and **--apikey** are required for every command issued to the CLI, see [how to set CLI profiles to store credentials in your machine](cli.md#profiles) to avoid typing them each time.
In the following commands will be omitted for sake of readability, but they are required.

## Tasks

```
Manage tasks

Usage:
   task [command]

Available Commands:
  artefacts   Show artefacts of a task
  attach      attach to a task output
  clone       clone a task
  create      Create a new task
  download    Download task artefacts
  execute     execute task
  list        List tasks
  log         Show log of a task
  monitor     Monitor tasks and propagate exit status
  remove      Remove a task
  show        Show a task
  start       Start a task
  stop        Stop a task

Flags:
  -h, --help   help for task

Global Flags:
  -k, --apikey string    Mottainai API key (default "fb4h3bhgv4421355")
  -m, --master string    MottainaiCI webUI URL (default "http://localhost:8080")
  -p, --profile string   Use specific profile for call API.

Use " task [command] --help" for more information about a command.
```


### Create

After [defining a task](tasksandpipelines.md) inside a yaml or json file you can use the CLI to create tasks and pipelines to be executed remotely
by the nodes belonging to your cluster.

    mottainai-cli task create --yaml file.yaml

### Attach

Once the task is created, the CLI will return an ID and instructions on how to interact with it.
If you want to see the logs of a running task and see updates, run:

    mottainai-cli task attach <taskid>


### Stop

To stop a running task:

    mottainai-cli task stop <taskid>

### Remove

To remove a task, and its associated artefacts:

    mottainai-cli task remove <taskid>


### Monitor

Wait for tasks to complete, and propagate error to the console :

    mottainai-cli task monitor <taskid> <taskid2> <taskid3> ...

### Log

Get task current log:

    mottainai-cli task log <taskid>

### Download

You can download a task artefacts with the CLI:

    mottainai-cli task download <id> <destination>


### List

List all tasks:

    mottainai-cli task list

### Clone

Generate a new task from an old one:

    mottainai-cli task clone <id>

## Profiles

```
Manage CLI Profiles

Usage:
   profile [command]

Available Commands:
  create      Create a new profile
  list        List profiles
  remove      Remove a profile

Flags:
  -h, --help   help for profile

Global Flags:
  -k, --apikey string    Mottainai API key (default "fb4h3bhgv4421355")
  -m, --master string    MottainaiCI webUI URL (default "http://localhost:8080")
  -p, --profile string   Use specific profile for call API.

Use " profile [command] --help" for more information about a command.
```

Profiles can be used to store permanently options that CLI uses to connect and authenticate to the Mottainai Server.

To create a new profile:

    mottainai-cli profile create <name> <instanceurl> <apikey>

Later, when you are using the cli you can specify

    mottainai-cli --profile <name> commmands...

See ```mottainai-cli profile --help``` for an overview.

To define a default profile, you can set the environment variable ```MOTTAINAI_CLI_PROFILE``` inside your .bashrc (or equivalent), in this way you can issue command directly, for example:

        mottainai-cli task attach <taskid>

## Namespace

```
Manage namespaces

Usage:
   namespace [command]

Available Commands:
  clone       clone a namespace
  create      Create a namespace
  delete      Delete a namespace
  download    Download namespace artefacts
  list        List namespaces
  show        Show artefacts belonging to namespace
  tag         Tag a namespace
  upload      Upload file to a namespace

Flags:
  -h, --help   help for namespace

Global Flags:
  -k, --apikey string    Mottainai API key (default "fb4h3bhgv4421355")
  -m, --master string    MottainaiCI webUI URL (default "http://localhost:8080")
  -p, --profile string   Use specific profile for call API.

Use " namespace [command] --help" for more information about a command.
```

### Create

Create a namespace:

    mottainai-cli namespace create <name>

To default, normal users have access only to namespaces which prefix with their username.

Administrators or Managers aren't subject to such restriction.

### Download

Download all files in a namespace

    mottainai-cli namespace download <name> <path>

### Upload

Upload a file in a namespace, note destination is in absolute form in respect of the namespace content (e.g. */* to push to the root of namespace):

    mottainai-cli namespace upload <name> <file> <destination>

### Delete

Delete namespace (permanently):

    mottainai-cli namespace delete <name>

### List

List all namespaces:

    mottainai-cli namespace list

### Clone

Generate a new namespace from an old one:

    mottainai-cli namespace clone <namespace>

### Tag

Publish task produced artefacts into a namespace:

    mottainai-cli namespace tag <name> --from <taskid>


## Storage

```
Manage storages

Usage:
   storage [command]

Available Commands:
  create      Create a storage
  delete      Delete a storage
  download    Download storage artefacts
  list        List storages
  show        Show artefacts belonging to a storage
  upload      Upload file to a storage

Flags:
  -h, --help   help for storage

Global Flags:
  -k, --apikey string    Mottainai API key (default "fb4h3bhgv4421355")
  -m, --master string    MottainaiCI webUI URL (default "http://localhost:8080")
  -p, --profile string   Use specific profile for call API.

Use " storage [command] --help" for more information about a command.
```

### Create

Create a storage:

    mottainai-cli storage create <id>

To default, normal users have access only to namespaces which prefix with their username (e.g. *user::foo*).

Administrators or Managers aren't subject to such restriction.

### Download

Download all files in a storage

    mottainai-cli storage download <id> <destination>

### Upload

Upload a file in a storage, note destination is in absolute form in respect of the storage content (e.g. */* to push to the root of namespace):

    mottainai-cli storage upload <id> <file> <destination>

### Delete

Delete storage (permanently):

    mottainai-cli storage delete <id>

### List

List all storages:

    mottainai-cli storage list
