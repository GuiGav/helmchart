# Pachyderm Helm Chart

The repo contains the pachyderm helm chart.

## Developer Guide
To run the tests for the helm chart, run the following:

```shell
$ make test
```

# Diff against `pachctl`

To see how this Helm chart and `pachctl deploy` differ, one can do
something similar to the following:

1. Generate a `pachctl` manifest with

    ```shell
    $ pachctl deploy googleBUCKET-NAME 10 --dynamic-etcd-nodes 1 -o yaml --dry-run > pachmanifest.yaml
    ```

1. Generate a Helm manifest with

    ```shell
    $ helm template -f examples/gcp-values.yaml ./pachyderm > helmmanifest.yaml
    ```

1. Visually diff the two.

# JSON Schema

We use [helm-schema-gen](https://github.com/karuppiah7890/helm-schema-gen)
to manage the JSON schema.  It can be installed with:

```shell
$ helm plugin install https://github.com/karuppiah7890/helm-schema-gen.git
```

When updating `values.yaml` please run the following to update the
json schema file.

```shell
$ cd pachyderm
$ helm schema-gen values.yaml > values.schema.json
```

# Validate Helm manifest

```shell
go install github.com/instrumenta/kubeval
kubeval helmmanifest.yaml
```

## Test Helm Policies with Conftest

[Conftest](https://github.com/open-policy-agent/conftest) is a framework to write tests against structured configuration data.

### Install Conftest

```shell
$ helm plugin install https://github.com/instrumenta/helm-conftest
```

### Run the tests

```shell
$ helm conftest pachyderm -f ./examples/gcp-values.yaml
```