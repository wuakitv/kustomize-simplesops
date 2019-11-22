# Kustomize SimpleSOPS

Kustomize plugin to decrypt SOPS secrets in templates.

## Introduction

In Kubernetes it's possible to manage sensitive data using the `Secret` object, however, the information is stored encoded in base64 making unsuitable for storing it in a git repository.

[SOPS](https://github.com/mozilla/sops) is a tool used to encrypt/decrypt files using KMS (in AWS, GCP or Azure) or PGP keys. It can be used to encrypt the Kubernetes manifests, but then they cannot be read by Kustomize.

This plugin allows to encrypt any data in the Kubernetes manifests so Kustomize is able to decrypt it.

## Getting Started

### Prerequisites

* Kustomize (tested with 3.2.1)
* SOPS (tested with 3.4.0)
* Bash and awk

### Install

First install the plugin into the Kustomize plugins directory:
```
mkdir -p ~/.config/kustomize/plugin/rakuten.tv/v1/simplesops
cp SimpleSOPS ~/.config/kustomize/plugin/rakuten.tv/v1/simplesops
chmod +x ~/.config/kustomize/plugin/rakuten.tv/v1/simplesops/SimpleSOPS
```

Configure Kustomize to use the plugin by adding the following in the `kustomize.yaml` file:
```
transformers:
- secrets_sops.yaml
```

Then create the `secrets_sops.yaml` file with the following content:
```
apiVersion: rakuten.tv/v1
kind: SimpleSOPS
metadata:
  name: secrets-sops
```

Now any SOPS encrypted file can be unencrypted by Kustomize

## Other Kustomize SOPS plugins

There are other Kustomize plugins designed to read sensitive data from SOPS encrypted files, but all of them are implemented via generators so it's not possible to encrypt data from patches.

They also suffer from [skew problems](https://github.com/kubernetes-sigs/kustomize/blob/master/docs/plugins/goPluginCaveats.md) because they are all Go plugins.

Some examples:

* https://github.com/viaduct-ai/kustomize-sops
* https://github.com/Agilicus/kustomize-sops
* https://github.com/inloco/sops-kustomize-generator-plugin
