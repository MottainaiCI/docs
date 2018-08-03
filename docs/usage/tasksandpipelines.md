# Tasks

Tasks can be defined in both json or yaml, it boils down to:

```
# Image used by the task player
image: "my/image"

# Script to be executed
script:
  - echo "hello world"

# Task type, for now only docker is available
task: docker_execute
```

Those are the required fields:

- **image**: it's the environment where your task is going to be run
- **script**: list of commands to execute
- **task**: it's the task type - for now only Docker is supported

There are more field to allow you to control better the lifespan of the task, or further define the environment. This is a full task definition:

```
# Image used by the task player
image: "my/image"

# Script to be executed
script:
  - echo "hello world"

# Task type, for now only docker is available
task: docker_execute

# Git repository that will be the context of our build
# The node that will execute the task will fetch the content and
# executes the commands provided
source: 'https://github.com/user/repo'

# Path relative to the git repository in which the commands will be executed into
directory: '/dir/'

# List of path to map inside the build environment from the host
binds:
  - /dev/loop-control:/dev/loop-control
  - /test

# Set to yes or any value to cache the image used to run the task, otherwise omit
cache_image: "yes"

# Set to yes or any value to wipe the cache for the task, otherwise omit
cache_clean: "yes"

# Custom image entrypoint ( first executed binary, defaults to /bin/bash )
entrypoint:
  - /bin/bash
  - -c

# List of variables in the build environment
environment:
  - FOO=42

# Delay task execution of 800s
eta: '800'

# Drain artefacts from the supplied namespace
namespace: "staging:development"

# Perform pruning commands after task execution (backend dependent), omit to not execute
prune: "yes"

# A specific queue where to send the task to.
queue: "amd64"

# Task id where to drain artefacts from
root_task: "34132345135151"

# Task associated storage
storage: "8925468933589065900"

# relative path for drained storages (defaults storage)
storage_path: storage

# relative path for drained artefacts from namespace/root_task (defaults artefacts)
artefact_path: artefacts

# On success, tag task's produced artefacts into a namespace (create if doesn't exists)
tag_namespace: staging:iso

# Specify artefacts publishing mode on namespaces. by default it replace namespace content during tag.
# - append: Do not replace namespace content, append to existing one
publish_mode: "append"

# Task time limit (in seconds)
timeout: 0
```

Most notably, all optional fields:


- **source**: Git repository where your source code is hosted. The node will fetch it and the task commands will be executed in the cloned repository
- **directory**: Define a directory inside your Git repository which will be where your sequence of commands will be executed
- **queue**: Specify a queue where to send the task to. If you setup your nodes to listen in different queues, you can exploit it to partition your cluster based on your preferences (e.g. architecture, host capabilities, etc. )
- **storage**: Storage ID which the task has access to - depending on the user permissions. See [Storage](storage.md) for more detail.
- **namespace**: Namespace is a unique string (as e.g. *moo*) to drain artefacts from. Already published artefacts can be shared in such way between different tasks in different hosts. See [Namespace](namespace.md) for more detail.
- **tag_namespace**: Namespace is a unique string (as e.g. *sheep*) where to tag task produced content automatically on success. See also [Namespace](namespace.md) for more detail.

# Pipelines

In order to automate furthermore your infrastructure, it's possible to define pipelines of tasks.
Tasks can be chained together to e.g. pass-by artifacts between each other, and if one of them fails, the chain is interrupted.
There are 3 kinds of pipelines: Groups, Chains and Chords.

## Groups

A group of task is a parallel group that is meant to run together.

```yaml
pipeline_name: "My Pipeline"
concurrency: 1 # Number of parallel builds
group:
  - "task1"
  - "task2"
  - "task3"

queue: 'general' # Optional, force the pipeline to a specific queue

tasks:
  task1:
    image: sabayon/base-amd64
    script:
      - echo 'hello'
    task: docker_execute
  task2:
    image: sabayon/base-amd64
    script:
      - echo 'hello 2!'
    task: docker_execute
  task3:
    script:
      - echo 'hello 3'
      - exit 1 # Make it fail!
    task: docker_execute
```

- **tasks** (required): A list of task definition in form ```key -> task```
- **group** (required): A list of task keys that are refering to the task definition
- **pipeline_name** (required): Name of the pipeline
- **concurrency** (optional): Number of parallel builds

## Chains

Chains are sequences of task, each one is started only if the predecessor in the chain succeeded.

```yaml
pipeline_name: "My Pipeline"
chain:
  - "task"
  - "task2"
  - "task3"

tasks:
  test:
    image: sabayon/base-amd64
    script:
      - echo 'hello'

    task: docker_execute
  task2:
    image: sabayon/base-amd64
    script:
      - echo 'hello 2!'
      # - exit 1 If this fails, then task3 is not executed.
    task: docker_execute
  task3:
    image: sabayon/base-amd64
    script:
      - echo 'hello from 3'
    task: docker_execute

```
As you can see the pipeline task field is a list of task that are indexed by a string. You can create keys freely and use them to define the execution order.

Relevant fields not seen already:

- **group** (required): A list of task keys that are refering to the task definition

## Chords

Execute a callback after a group ends successfully:

```yaml
pipeline_name: "My Pipeline"

group:
  - "task1"
  - "task2"
chord:
  - "task3"

tasks:
  task1:
    image: sabayon/base-amd64
    script:
      - echo 'hello'
    task: docker_execute
  task2:
    image: sabayon/base-amd64
    script:
      - echo 'hello world!'
      # - exit 1 If it fails, task3 is not executed
    task: docker_execute
  task3:
    image: sabayon/base-amd64
    script:
      - echo 'hello from 3'
    task: docker_execute
```

Relevant fields not seen already:

- **chord**: A list of task executed if chord is successfull (currently, just one callback is supported)
