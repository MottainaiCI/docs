# Storage

Storages are virtual buckets where it is possible to upload content from the [CLI](cli.md#storage) or web-interface, that can be retrieved from the task during the execution.

Storages are referenced in the task fields by ID, and you can use them to e.g. provide separate files that cannot be shared in the git repositories and that are not subject to public access.

Users can create storages which prefixes with their name, while administrators can create storages of any kind.

To create a new storage, use the CLI:

    mottainai-cli storage create foo

An ID is returned from the CLI, which you can later use in the tasks definitions to reference to the storage.

    $ mottainai-cli storage create test
     3406173479640723926

In the task definition, annotate the ID in the ```storage``` field. Inside the task script now you can reference to the storage
in the ```storage/``` directory relative to your task execution, e.g.:


```yaml
script:
  - ls storage/
  - source storage/.secrets.env
  - ...
```

Storages are not uploaded from an agent node, they just are read-only for workers.

## Upload

You can upload files from the cli to your storage.

    $ mottainai-cli storage upload <ID> <file_to_upload> <storage_path>

The ``ID`` is the identifier of the storage, ``file_to_upload`` is the path of the file you wish to upload, and ```storage_path``` is the absolute path relative to the storage where it should be moved to, e.g. if we have a hello_world.txt in the current dir:

    $ mottainai-cli storage upload 3406173479640723926 hello_world.txt /hello_world.txt

In the task which is referencing it, hello_world.txt now is accessible:

```yaml
script:
  - ls storage/
  - cat storage/hello_world.txt
  - ...
```
