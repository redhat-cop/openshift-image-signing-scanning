node-config
=============

Configures an OpenShift node to support image signatures. 

## Role Variables

This role contains a number of variables that drive the specific execution. The following variables are key to the functionality of this role:

|Variable|Description|
|--------|-----------|
|`local_gpg_publickey`| Location of the public key to configure in OpenShift |
|`ocp_registry`| Location of the OpenShift registry |
