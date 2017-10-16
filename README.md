OpenShift Container Platform Image Signing and Image Scanning
========================================

_This repository is currently undergoing active development. Functionality may be in flux_

## Overview

The OpenShift Container Platform ecosystem contains mechanisms for securely managing container images. This includes but is not limited to [image signing](https://docs.openshift.com/container-platform/3.6/admin_guide/image_signatures.html) and [image scanning](https://docs.openshift.com/container-platform/3.6/security/container_content.html#security-content-scanning).

The goal of this repository is to contain tools necessary to perform these taks within OpenShift. 

## Prerequisites

To facilitate image signing and scanning, the following prerequisite sections must be completed.

### Create a GPG Keypair

A GPG Keypair is required in order to sign and validate images. Execute the following command to create a new keypair:

```
gpg2 --gen-key
```

**Note: Only keys without a passphrase are currently supported.**

Make a note of the location of the key pair as it will be needed in subsequent steps.

## Base Infrastructure

Several steps must be completed in order to satisfied the necessary access and permissions within the OpenShift cluster. By default, the infrastructure components are designed to run within the _openshift-infra_ project. Templates are available that allow the destination to be customized if necessary. 

A user with `cluster-admin` privileges must be used to configure the base infrastructure components as they modify cluster level resources.

Create a new service account and assign it _cluster-admin_ privileges along with adding it to the _privileged_ SCC. 

```
oc process -f policy/sa-rolebinding-template.yml | oc apply -f-
oc adm policy add-scc-to-user privileged  -n openshift-infra -z imagemanager
```
 

## Base Image

Image signing and image scanning actions each utilizes the [Atomic Command Line Tool](https://github.com/projectatomic/atomic). To support running within OpenShift, a base container is available within the [image-sign-scan-base](image-sign-scan-base) folder.

To process and instantiate the template the base image components, execute the following command:

```
oc process -f image-sign-scan-base/image-sign-scan-base.yml | oc apply -f-
```

A new image will be created as a foundational component for subsequent sections 

## Image Signing

This section describes how to setup an OpenShift environment to support the signing of images

### Add GPG Secret

To support image signing, a GPG key pair was previously created. The files produced will be used in an Secret and injected into the signing process.

Either reference the folder containing the GPG components or copy the files to a new directory. Execute the following command to create the secret:

```
oc secrets new gpg <folder_containing_resources>
```

### Create a job to Sign an Image

A template to create a job is available to sign an image within the OpenShift registry and available in the _sign-image_ folder.

The template requires the following parameters:

|Template Parameter|Description|Default Value|
|--------------------------|----------------|-----------------|
|SERVICE_ACCOUNT_NAME|Name of the Service Account to use|imagemanager|
|NAMESPACE|Namespace in which to create the job|openshift-infra|
|IMAGE_TO_SIGN|Image location (Ex: _docker-registry.default.svc:5000/test-project/test-image:latest_)| |
|SIGN_BY|Identity of the signer| |
|GPG_SECRET| Name of the secret containing the GPG keypair| |

Instantiate the template

```
oc process -p <param>... -f sign-image/sign-image-template.yml | oc apply -f-
```

## Image Scanning

Coming soon