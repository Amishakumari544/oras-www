# Manifest Annotations

[Annotations](https://github.com/opencontainers/image-spec/blob/master/annotations.md),
which are supported by [OCI Image Manifest](https://github.com/opencontainers/image-spec/blob/master/manifest.md#image-manifest)
and [OCI Content Descriptors](https://github.com/opencontainers/image-spec/blob/master/descriptor.md),
are also supported by `oras`.

## Make annotations

Users can make annotations to the manifest, the config, and individual files (i.e. layers) by the `--annotation-file file` option.
The annotations file is a JSON file with the following format:

```json
{
  "<filename>": {
    "<annotation_key>": "annotation_value"
  }
}
```

There are two special filenames / entries:
- `$config` is reserved for the annotation of the manifest config.
- `$manifest` is reserved for the annotation of the manifest itself.

For instance, the following annotation file `annotations.json`:

```json
{
  "$config": {
    "hello": "world"
  },
  "$manifest": {
    "foo": "bar"
  },
  "cake.txt": {
    "fun": "more cream"
  }
}
```

Running the following command

```
oras push --annotation-file annotations.json localhost:5000/club:party cake.txt juice.txt
```

results in

```json
{
  "schemaVersion": 2,
  "config": {
    "mediaType": "application/vnd.oci.image.config.v1+json",
    "digest": "sha256:44136fa355b3678a1146ad16f7e8649e94fb4fc21fe77e8310c060f61caaff8a",
    "size": 2,
    "annotations": {
      "hello": "world"
    }
  },
  "layers": [
    {
      "mediaType": "application/vnd.oci.image.layer.v1.tar",
      "digest": "sha256:22af0898315a239117308d39acd80636326c4987510b0ec6848e58eb584ba82e",
      "size": 6,
      "annotations": {
        "fun": "more cream",
        "org.opencontainers.image.title": "cake.txt"
      }
    },
    {
      "mediaType": "application/vnd.oci.image.layer.v1.tar",
      "digest": "sha256:be6fe11876282442bead98e8b24aca07f8972a763cd366c56b4b5f7bcdd23eac",
      "size": 7,
      "annotations": {
        "org.opencontainers.image.title": "juice.txt"
      }
    }
  ],
  "annotations": {
    "foo": "bar"
  }
}
```
