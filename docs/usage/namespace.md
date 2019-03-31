# Namespace

Namespaces allows you to publish and manage content that is subject to a release process.

Artefacts produced from [Tasks and Pipelines](tasksandpipelines.md) can be organized into namespaces.

Namespaces are accessible from the web interface:

    https://your.instance.com/namespace/[namespace name]

Only administrators are allowed (for now) to publish in top layer ones, standard users have access only to namespaces which prefixes with their username.

## Create

You can create a new namespace from the CLI with:

    $> mottainai-cli namespace create new-namespace

On the task (or pipeline) definition [you can publish the resulting artefact](tasksandpipelines.md) to the namespace by adding ```tag_namespace: new-namespace```.

## Clone

You can create a new namespace cloning from an old one:

    $> mottainai-cli namespace clone --from old-namespace new-namespace

## Delete

You can delete a namespace from the CLI with:

    $> mottainai-cli namespace delete some-namespace

## Remove

You can remove files from a namespace from the CLI with:

    $> mottainai-cli namespace remove some-namespace /path/to/file


e.g. if the *foobar* namespace have a file at the top level called 'hello.txt', you can remove it with:

    $> mottainai-cli namespace remove foobar /hello.txt
