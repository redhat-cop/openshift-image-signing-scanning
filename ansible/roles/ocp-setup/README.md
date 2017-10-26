ocp-setup
==========

Configures an OpenShift cluster to support image signing.

# Overview

To support image signing within an OpenShift environment, the following actions are configured:

* Labeling nodes that should run image signing processes
* Taint nodes responsible for signing images
* Creation of cluster resources (roles, service accounts)
    * New project created for containing image signing resources
    * Cluster roles
    * Build a custom image signing builder image
    * Template for spawning job to create signed images

# Role Variables

The following variables help drive the execution of the role

|Variable|Description|Default Value|
|--------|-----------|-------------|
|`openshift_signer_project_name`| Project Name Containing Build Components | `image-signer` |
|`openshift_signer_node_label`| Label applied to Nodes Delegated for Image Signing | `image_signer=true` |
|`openshift_signer_node_taint`| Taint applied to OpenShift Image Signing Nodes | `image_signer=true:NoExecute` |
|`openshift_signer_gpg_secret_name` | Secret name containing GPG keys | `gpg` |
|`openshift_signer_sa_name` | Name of the service account running Image Signing Processes | `imagemanager` |

