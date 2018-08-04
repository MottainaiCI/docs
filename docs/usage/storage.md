# Storage

Storages are virtual buckets where it is possible to upload content from the [CLI](cli.md#storage) or web-interface, that can be retrieved from the task during it's execution.

Storages are referenced in the task fields by ID, and you can use them to e.g. provide separate files that cannot be shared in the git repositories and that are not subject to public access.

Users can create storages which prefixes with their name, while administrators can create storages of any kind.
