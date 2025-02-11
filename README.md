# Buildpacks Catalog

![Test Workflow](https://github.com/kadras-io/buildpacks-catalog/actions/workflows/test.yml/badge.svg)
![Release Workflow](https://github.com/kadras-io/buildpacks-catalog/actions/workflows/release.yml/badge.svg)
[![The SLSA Level 3 badge](https://slsa.dev/images/gh-badge-level3.svg)](https://slsa.dev/spec/v1.0/levels)
[![The Apache 2.0 license badge](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

> **Warning**
> This package has been archived together with the kpack package. If you need a build service, you can choose GitHub Actions, supported by Kadras, or bring your own build service package.

A Carvel package providing a set of buildpacks, stacks, and builders to use with [kpack](https://github.com/kadras-io/package-for-kpack), a Kubernetes-native implementation of [Cloud Native Buildpacks](https://buildpacks.io) to build application source code into OCI images.

This package relies on the [Paketo Buildpacks](https://paketo.io) implementation, which provide support for multiple languages and frameworks, including Java, Spring, GraalVM, Go, Python, NodeJs, and more.

## 🚀&nbsp; Getting Started

### Prerequisites

* Kubernetes 1.29+
* Carvel [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl) CLI.
* Carvel [kapp-controller](https://carvel.dev/kapp-controller) deployed in your Kubernetes cluster. You can install it with Carvel [`kapp`](https://carvel.dev/kapp/docs/latest/install) (recommended choice) or `kubectl`.

  ```shell
  kapp deploy -a kapp-controller -y \
    -f https://github.com/carvel-dev/kapp-controller/releases/latest/download/release.yml
  ```

### Dependencies

Buildpacks Catalog requires the [kpack](https://github.com/kadras-io/package-for-kpack) package. You can install it from the [Kadras package repository](https://github.com/kadras-io/kadras-packages).

### Installation

Add the Kadras [package repository](https://github.com/kadras-io/kadras-packages) to your Kubernetes cluster:

  ```shell
  kctrl package repository add -r kadras-packages \
    --url ghcr.io/kadras-io/kadras-packages \
    -n kadras-system --create-namespace
  ```

<details><summary>Installation without package repository</summary>
The recommended way of installing the buildpacks-catalog package is via the Kadras <a href="https://github.com/kadras-io/kadras-packages">package repository</a>. If you prefer not using the repository, you can add the package definition directly using <a href="https://carvel.dev/kapp/docs/latest/install"><code>kapp</code></a> or <code>kubectl</code>.

  ```shell
  kubectl create namespace kadras-system
  kapp deploy -a buildpacks-catalog-package -n kadras-system -y \
    -f https://github.com/kadras-io/buildpacks-catalog/releases/latest/download/metadata.yml \
    -f https://github.com/kadras-io/buildpacks-catalog/releases/latest/download/package.yml
  ```
</details>

Install the Buildpacks Catalog package:

  ```shell
  kctrl package install -i buildpacks-catalog \
    -p buildpacks-catalog.packages.kadras.io \
    -v ${VERSION} \
    -n kadras-system
  ```

> **Note**
> You can find the `${VERSION}` value by retrieving the list of package versions available in the Kadras package repository installed on your cluster.
> 
>   ```shell
>   kctrl package available list -p buildpacks-catalog.packages.kadras.io -n kadras-system
>   ```

Verify the installed packages and their status:

  ```shell
  kctrl package installed list -n kadras-system
  ```

## 📙&nbsp; Documentation

Documentation, tutorials and examples for this package are available in the [docs](docs) folder.
For documentation specific to kpack, check out [github.com/buildpacks-community/kpack](https://github.com/buildpacks-community/kpack).

## 🎯&nbsp; Configuration

The Buildpacks Catalog package can be customized via a `values.yml` file.

  ```yaml
  kp_default_repository:
    name: ghcr.io/thomasvitale/kpack-build
  ```

Reference the `values.yml` file from the `kctrl` command when installing or upgrading the package.

  ```shell
  kctrl package install -i buildpacks-catalog \
    -p buildpacks-catalog.packages.kadras.io \
    -v ${VERSION} \
    -n kadras-system \
    --values-file values.yml
  ```

### Values

The Buildpacks Catalog package has the following configurable properties.

<details><summary>Configurable properties</summary>

| Config | Default | Description |
|-------|-------------------|-------------|
| `kp_default_repository.name` | `""` | The default repository to use for builder images and dependencies. For example, GitHub Container Registry: `ghcr.io/my-org/my-repo`; GCR: `gcr.io/my-project/my-repo`; Harbor: `myharbor.io/my-project/my-repo`, Dockerhub: `docker.io/my-username/my-repo`.|

</details>

## 🛡️&nbsp; Security

The security process for reporting vulnerabilities is described in [SECURITY.md](SECURITY.md).

## 🖊️&nbsp; License

This project is licensed under the **Apache License 2.0**. See [LICENSE](LICENSE) for more information.
