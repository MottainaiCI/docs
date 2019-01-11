# Artefacts

In Mottainai, tasks can optionally produce *Artefacts* that you can later reference and tag into specific [namespaces](namespace.md) ( or here referred also as bucket ).

In a task definition, you can specify to tag automatically on success into a namespace by setting *tag_namespace*:

```
# Create an empty file as artefact

script:
- touch artefacts/hello.txt

tag_namespace: 'my-bucket-name'
```

You can also map the artefact folder to a custom one, by defining *artefact_path*.

```
artefact_path: out
```

And use it like this in our script definition:

```
script:
- touch out/hello.txt

artefact_path: out
tag_namespace: 'my-bucket-name'
```

## Append file to the bucket

By 'tagging' a namespace with the task, we actually replace the bucket content with the task artefacts, but this is not always the case.

We can define *publish_mode* in the task definition to accomplish that:

```
# Specify artefacts publishing mode on namespaces. by default it replace namespace content during tag. # - append: Do not replace namespace content, append to existing one 
publish_mode: "append"
```

## Get data from a bucket

To retrieve data during our task execution instead, specify *namespace* in the task definition:

```
script:
- ls artefacts/hello.*
namespace: 'some-bucket'
```

In this way, we can retrieve the same data belonging to the bucket, allowing to pass-by the content between different tasks, even from different task types.

## Manually tag artefacts

Using the [CLI](cli.md) it's possible to tag tasks artefacts into new namespaces and also merging and replacing their content.

Use the task id which you want to use as a source for the namespace content, e.g. 

```
mottainai-cli namespace tag some-bucket --from 123456
```

Will tag the artefacts produced by *123456* into the *some-bucket* namespace.




