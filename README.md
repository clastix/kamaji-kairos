# Kamaji and Kairos

Run immutable worker nodes managed by Kairos, joining a Tenant Control Plane powered by Kamaji thanks to the [`kubeadm` Kairos provider](https://github.com/kairos-io/provider-kubeadm)

## Kairos Kubeadm provider OCI artefact (k8s v1.31.1)

Clone the [kairos-io/provider-kubeadm](https://github.com/kairos-io/provider-kubeadm) repository,
and build artefacts according to your needs.

For our use case, we're going to build on Ubuntu 22 LTS amd64 to install all the required packages for a Kubernetes v1.31.1 worker node.

```
$: earthly +docker -output +build-provider-package --BASE_IMAGE=quay.io/kairos/core-ubuntu-22-lts:latest --IMAGE_REPOSITORY=quay.io/clastix --KUBEADM_VERSION=1.31.1

## redacted

Image github.com/kairos-io/provider-kubeadm:main+docker output as quay.io/clastix/core-ubuntu-22-lts-kubeadm:1.31.1
Image github.com/kairos-io/provider-kubeadm:main+docker output as quay.io/clastix/core-ubuntu-22-lts-kubeadm:1.31.1_v0.0.0-4675af22
```

Push your image to your container registry

> Image must be public: check out the Kairos documentation for
> [private registries authentication](https://kairos.io/docs/advanced/private_registry_auth/).

## Build Kairos reset image as OCI artefact

Refer to the `aws.yaml` file in this repository: it must be fed as an AWS user-data configuration.

> Customize the `cluster` section with your `kubeadm` parameters, such as:
>
> - token
> - API Endpoint
> - role (with Kamaji acting as Control Plane manager, it's `worker`)
> - optionally, any custom configuration
