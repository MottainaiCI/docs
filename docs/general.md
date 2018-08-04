# General concepts

Mottainai is composed of 4 main components:

- Mottainai Server - central node to dispatch tasks and collect artefacts from build
- Mottainai CLI - Application to interface with Mottainai Server
- Mottainai Agent - Runtime for nodes that are executing tasks
- Mottainai Bridge - Suite of library/binaries to listen on events emitted by the infrastructure

## Server
The [Server](https://github.com/MottainaiCI/mottainai-server) is the main compontent which orchestrate builds across nodes: it dispatches tasks to the nodes using a broker implementation and it take cares of handling the artefacts from the build ( any kind of produced content from your build ). You can customize more the infrastructure behind it,
and setup Ceph or other storage engines for permanent and distributed storage.

## Agent
The [Agent](https://github.com/MottainaiCI/mottainai-agent) is the main software which is executing the tasks, is being run by nodes belonging to the cluster. Among its duties, it takes care of executing the task in the specified environment and communicate to the Server the node status.

## CLI
[Command Line Interface](https://github.com/MottainaiCI/mottainai-cli) to interact with the cluster, to manage and publish the artefacts. It's a mere wrapper over the REST API. [See the CLI doucmentation for an overview](usage/cli.md).

## Bridge
[Standalone/Library](https://github.com/MottainaiCI/mottainai-bridge) suitable for creating custom hooks to listen at infrastructure events (e.g. IRC notifications when a new task is created, etc. )

# Tasks, Pipelines and Plans

Tasks are the core concept of Mottainai. They define an environment where to execute a set of command, and they can produce content that is meant to be re-distributed later (in private, or in public). [You can see a brief syntax overview here](usage/tasksandpipelines.md).


# Storages, Artefacts, Namespaces

In Mottainai, every task can produce a set of *artefacts*.
Artefacts can be later collected into so-called **namespaces**. Namespaces can be configured for fine-grained access (public, organization, private, internal, feature still [in development](https://github.com/MottainaiCI/mottainai-server/issues/12)) and collect artefacts. The cli allows also to tag their content manually, which allows rollbacks and helps in maintenance.


# Persistent caching and image cache registries

It's possible, if the backend supports it, to have persistent image caches across the cluster by setting up a private registry (e.g. Docker) or by using even dockerhub itself. When this feature is enabled per-task, the build environment will be carried over between different clusters, incrementally updating itself.

# Artefacts maintenance

Tag, remove, download, upload and publish artefacts from your task within the same CLI or via REST API - you can even configure your infrastructure to deliver the data in different medium ( rsync, git, etc. ) but you can still manage the content from the same interface.
