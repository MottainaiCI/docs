# Welcome to Mottainai docs

Build powerful, flexible and decentralized pipelines playable locally.
Manage, Publish and release your task's produced content.

Mottainai is a Task Distributed, Continous Integration and Delivery system - it allows you to build, test, deploy and manage
content built from custom tasks from different nodes in a network. You can hook specific tasks to Git repositories in a CI style, or
either directly execute pipelines or production tasks in safe environments.

It supports different brokering backends: AMQP, Redis, Memcache, AWS SQS, DynamoDB, Google Pub/Sub and MongoDB.

It was developed for building and releasing packages for Linux Distributions,
it is used by [Sabayon Linux](https://www.sabayon.org) to produce and orchestrate builds of community repositories and to build LiveCDs -
but it's suitable for every workflow which is artefact-oriented.
For more, see also the [Official Gentoo project page](https://wiki.gentoo.org/wiki/Project:Build_Service).

*Warning*: This is still alpha software - API is not stable and subject to constant changes still.

# Getting started

First steps to start to build!

- [How it works?](general.md)
- [Setup](setup.md)
- [Tasks and pipelines](usage/tasksandpipelines.md)
- [Plans](usage/plans.md)
- [Powerful CLI to the rescue!](usage/cli.md)
