# 1. Ansible Receptor Collection

- [1. Ansible Receptor Collection](#1-ansible-receptor-collection)
- [2. Description](#2-description)
- [3. Environments Tested](#3-environments-tested)
- [4. Roles](#4-roles)
  - [4.1. Podman](#41-podman)
    - [4.1.1. Variables](#411-variables)
  - [4.2. Setup](#42-setup)
    - [4.2.1. Variables](#421-variables)
- [5. License](#5-license)


# 2. Description
This collection prepare and configure endpoint to execute podmand and receptor.
It's used by AWX into instance bundle for configure endpoint into AWX.

This collection contains 2 roles:
- __Podman__ : that install and configure podman on the endpoint
- __Setup__: that will install and configure receptor on the endpoint.

<br>

# 3. Environments Tested

This ollection has been tested on following OS:
| OS | Release | Tested (Y/N)  | Comment |
| ---------- | ------- | ---------- | -------- |
| Centos | >=8 | Y | |
| Redhat | >=8 | Y | |
| Ubuntu | >=20 | Y | |

<br>

# 4. Roles

<br>

## 4.1. Podman

Installs and configures Podman.

<br>

### 4.1.1. Variables

| Parameter | Type | Defaults | Comments |
| ---------- | ------- | ---------- | -------- |
| __podman_user__  | string | `podman` | The user under which podman will be configured. |
| __podman_group__  | string | `podman` | The group under which podman will be configured. |
| __default_runtime__  | string | `crun` | The default container runtime to use for Podman. |
| __default_cgroup_manager__  | string | `cgroupfs` | The default cgroup manager to use for Podman. |

<br>

## 4.2. Setup

Installs and configures a Receptor node.
On Centos/Redhat/Fedora it can be used the dnf package already bundled.
On Debian you can set `receptor_install_method` to `release` in that way we can download pre-build package created by github action.

If you want to use your pre-build local package, you can set `receptor_install_method` to `local` and role will use it (based on other variables).
In case of `receptor_install_method` is `local` or `release`, role will configure the receptor service in the same way of Centor/Redhat package.


<br>

### 4.2.1. Variables

| Parameter | Type | Defaults | Comments |
| ---------- | ------- | ---------- | -------- |
| __receptor_packages__ | list | `[ "receptor" ]` Centos/Redhat <br> `[]` Debian | Set the names of the packages needed to install Receptor. On Debian, receptor package not exists, so it will be a empty list |
| __receptor_dependencies__ | list | `[]` | Specify other packages needed, probably on a per-node-type basis using groupvars or hostvars |
| __receptor_user__ | string | `receptor` | The user under which Receptor will be configured. |
| __receptor_group__ | string | `receptor` | The group under which Receptor will be configured. |
| __receptor_socket_dir__ | string | `/var/run/receptor` | The directory that Receptor will place its control socket into. |
| __receptor_control_filename__  | string | `receptor.sock` | The name of the control socket file. |
| __receptor_config_dir__ | string | `/etc/receptor` | Path to the Receptor config file. |
| __routable_hostname__ | string | `''` | Hostvar for the routable address to this node.  If this is unset `ansible_host` will be used instead.  Must be unique. Defaults to not set. |
| __receptor_peers__ | list of dict | `''` | Hostvar for the Ansible hosts that this node is peering outwards to. This is expected to be a list of dicts. In the dicts, the `'host'` key is required, `'port'` and `'protocol'` are optional and will default to the overall defaults for `receptor_port` and `receptor_protocol`. |
| __receptor_tls__ | boolean | `false` | If set to true, this forces the minimum TLS version used to be 1.3. Otherwise, the minimum version will be 1.2.  This variable has no effect unless `receptor_tls` is enabled. |
| __receptor_mintls13__ | boolean | `false` | If set to true, this forces the minimum TLS version used to be 1.3. Otherwise, the minimum version will be 1.2.  This variable has no effect unless `receptor_tls` is enabled. |
| __receptor_tls__ | boolean | `false` | If set to true, this forces the minimum TLS version used to be 1.3. Otherwise, the minimum version will be 1.2.  This variable has no effect unless `receptor_tls` is enabled. |
| __receptor_tls_dir__ | string | `/etc/receptor/tls` | Directories on the server where the TLS keys would be located. |
| __receptor_tls_ca_dir__ | string | `{{ receptor_tls_dir }}/ca` | Directories on the server where the CA keys would be located. |
| __receptor_tls_certfile__ | string | `{{ receptor_tls_dir }}/{{ receptor_host_identifier }}.crt` | Path on the server to the public and private TLS key files. |
| __receptor_tls_keyfile__ | string | `{{ receptor_tls_dir }}/{{ receptor_host_identifier }}.key` | Path on the server to the public and private TLS key files. |
| __receptor_ca_certfile__ | string | `"{{ receptor_tls_ca_dir }}/mesh-CA.crt"` | Path on the server where the public and private Certificate Authority key files would be located. |
| __receptor_ca_keyfile__ | string | `{{ receptor_tls_ca_dir }}/mesh-CA.key` | Path on the server where the public and private Certificate Authority key files would be located. |
| __custom_ca_certfile__ | string | `''` | Path on the local filesystem to user-provided Certificate Authority files. Defaults to not set. |
| __custom_ca_keyfile__ | string | `''` | Path on the local filesystem to user-provided Certificate Authority files. Defaults to not set. |
| __custom_tls_certfile__ | string | `''` | Hostvar that is the path on the local filesystem to user-provided per-node certificate files.  If used, both must be provided in combination with a `custom_ca_certfile` that was used to sign them. Defaults to not set. |
| __custom_tls_keyfile__ | string | `''` | Hostvar that is the path on the local filesystem to user-provided per-node certificate files.  If used, both must be provided in combination with a `custom_ca_certfile` that was used to sign them. Defaults to not set. |
| __receptor_sign__ | boolean | `false` | If set to true, host will sign any work that it sends over the Receptor mesh. |
| __receptor_verify__ | boolean | `false` | If set to true,  host will verify any work that it receives using a public key. |
| __receptor_worksign_key_dir__ | string | `/etc/receptor` | Base dir on the server to the public and private OpenSSL work signing key files. |
| __receptor_worksign_private_keyfile__ | string | `{{ receptor_worksign_key_dir }}/work_private_key.pem` | Path on the server to the private OpenSSL work signing key files. |
| __receptor_worksign_public_keyfile__ | string | `{{ receptor_worksign_key_dir }}/work_public_key.pem` | Path on the server to the public OpenSSL work signing key files. |
| __custom_worksign_private_keyfile__ | string | `''` | Path on the local filesystem to user-provided OpenSSL work signing key files. Defaults to not set. |
| __custom_worksign_public_keyfile__ | string | `''` | Path on the local filesystem to user-provided OpenSSL work signing key files. Defaults to not set. |
| __receptor_fd_limit_soft__ | integer | `4096` | The file descriptor limits in PAM for Receptor. |
| __receptor_fd_limit_soft__ | integer | `8192` | The file descriptor limits in PAM for Receptor. |
| __receptor_app_service__ | string | `''` | Optional variable to tie Receptor together with some other service in systemd. Defaults to not set. |
| __receptor_log_level__ | string | `info` | The level at which Receptor should write logs.  Allowable options are 'error', 'warning', 'info', and 'debug'. |
| __receptor_app_service__ | string | `''` | Optional variable to tie Receptor together with some other service in systemd. Defaults to not set. |
| __receptor_listener__ | boolean | `true` | If set to true, disable Receptor to listen for incoming remote connections. |
| __receptor_local_only__ | boolean | `false` | If set to true, Receptor listen for local-only connections, this will take precedence over the valueof `receptor_listener`. |
| __receptor_protocol__ | string | `tcp` | Set the protocol this instance of Receptor will use. Allowable options are 'tcp', 'udp', and 'ws' for websockets) |
| __receptor_port__ | integer | `27199` | Set the port number used by this instance of Receptor. |
| __receptor_work_commands__ | dict | `''` | The definition of the Receptor work commands.  This variable is expected to be a dictionary, with keys the unique worktype name, andvalues a dict of the rest of the key-value pairs of the workdefinition. See [https://receptor.readthedocs.io/en/latest/workceptor.html](https://receptor.readthedocs.io/en/latest/workceptor.html) for moreinformation. |
| __receptor_kubernetes_commands__ | dict | `''` | The definition of the Receptor work-kubernetes commands.  This variable is expected to be a dictionary, with keys the unique worktype name, and values a dict of the rest of the key-value pairs of the work definition. See [https://receptor.readthedocs.io/en/latest/k8s.html](https://receptor.readthedocs.io/en/latest/k8s.html) for more information. |
| __receptor_install_method__ | string | `package` | Options are 'package', 'local', or 'release' If 'package', will use the os-specific package manager to install receptor. If 'local', will upload a local receptor binary. To be paired with `local_receptor_bin_file`. If 'release', the receptor binary will be downloaded from receptor Releases on github. |
| __local_receptor_bin_file__ | string | `/tmp/receptor-bin` | Path of local receptor binary. |
| __receptor_install_dir__ | string | `/usr/bin` | Receptor binary path on remote node. |
| __receptor_log_dir__ | string | `/var/log/receptor` | Receptor log directory. Used only when `receptor_install_method` is local or release. |
| __receptor_github_owner__ | string | `ansible` | Owner of github repo where we search and download package if `local_receptor_bin_file` not exists. |
| __receptor_github_repo__ | string | `receptor` | Repository github where we search and download package if `local_receptor_bin_file` not exists. |
| __receptor_github_release__ | string | `''` | Repository version where we search and download package if `local_receptor_bin_file` not exists. Defaults to not set. |
| __receptor_service_name__ | string | `receptor` | Name of systemd service that runs receptor. Used only when `receptor_install_method` is local or release. |

# 5. License

Apache 2

<br>
<br>
