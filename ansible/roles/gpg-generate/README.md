gpg-generate
============

Role to generate GPG Keys to sign and validate images

_Note_: This role does enable the Extra Packages for Enterprise Linux (EPEL) repository on the machine used to generate GPG keys

# Overview

Generates GPG keychains and exports the public key. Transfers these assets to the control node.

# Role Variables

The following variables dictate the execution of the role:

|Variable|Description|Default Value|
|--------|-----------|-------------|
|`gpg_key_gen_user_name`| GPG Username | `openshift` |
|`gpg_key_gen_user_email` | GPG Email Address (Also used for identity) | `openshift@example.com` |
| `gpg_key_gen_length` | GPG Key Length | `2048` |
|`gpg_key_gen_passphrase_enable` | Whether to enable a passphrase on the key (Should remain false) | `false` |
|`gpg_local_base_dir`|Location where to store GPG Keys fetched from remote host | `{{ playbook_dir }}/gpg_remote`|
