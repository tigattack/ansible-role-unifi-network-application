# Ansible Role: unifi_network_application

[![Build Status][build_badge]][build_link]
[![Ansible Galaxy][galaxy_badge]][galaxy_link]

Deploy [UniFi Network application]() in Docker

Install the role: `ansible-galaxy role install tigattack.unifi_network_application`

See [Example Playbooks](#example-playbooks) below.

## Prerequisites

* Docker. I recommend the [geerlingguy.docker](https://github.com/geerlingguy/ansible-role-docker) role.
* [community.docker](https://galaxy.ansible.com/ui/repo/published/community/docker/) Ansible collection. See [requirements.yml](requirements.yml).
* A chosen data path on the host.

## Role Variables

> [!TIP]
> Once installed, you can run `ansible-doc -t role tigattack.unifi_network_application` to see role documentation.

### `unifi_network_application_base_path`

| Type | Default      |
|------|--------------|
| path | `/opt/unifi` |

Base path for UniFi Network application data on the host.

### `unifi_network_application_app_version`

| Type   | Default  |
|--------|----------|
| string | `latest` |

Docker image version for the UniFi Network application.

### `unifi_network_application_mongo_version`

| Type   | Default |
|--------|---------|
| string | 7.0     |

Docker image version for MongoDB.

### `unifi_network_application_user`

| Type | Default |
|------|---------|
| raw  | `1000`  |

User ID to run the containers as.

### `unifi_network_application_group`

| Type | Default |
|------|---------|
| raw  | `1000`  |

Group ID to run the containers as.

### `unifi_network_application_docker_network`

| Type   | Default |
|--------|---------|
| string | `unifi` |

Name of the Docker network to connect the containers to.

### `unifi_network_application_mongo_dbname`

| Type   | Default |
|--------|---------|
| string | `unifi` |

MongoDB database name for UniFi Network application.

### `unifi_network_application_mongo_user`

| Type   | Default |
|--------|---------|
| string | `unifi` |

MongoDB username for UniFi Network application.

### `unifi_network_application_mongo_password`

| Type   | Default |
|--------|---------|
| string | None    |

MongoDB password for the UniFi Network application.

### `unifi_network_application_mongo_root_password`

| Type   | Default |
|--------|---------|
| string | None    |

MongoDB root password for the UniFi Network application.

### `unifi_network_application_timezone`

| Type   | Default   |
|--------|-----------|
| string | `Etc/UTC` |

Timezone for the UniFi Network application.

See https://en.wikipedia.org/wiki/List_of_tz_database_time_zones#List.

### `unifi_network_application_app_memlimit`

| Type | Default |
|------|---------|
| int  | `1024`  |

Memory limit (in MB) for the UniFi Network application container.

### `unifi_network_application_deployment_always_pull`

| Type | Default |
|------|---------|
| bool | `false` |

Always pull the UniFi Network application and MongoDB images.

This can be useful to ensure you're always up to date if the images aren't pinned to a specific version (e.g. `latest`).

### `unifi_network_application_deployment_wait_for_health`

| Type | Default |
|------|---------|
| bool | `true`  |

Wait for the MongoDB and UniFi Network application containers to be healthy before continuing.

### `unifi_network_application_prune_images`

| Type | Default |
|------|---------|
| bool | `false` |

Prune unused Docker images after deployment.

> [!WARNING]
> This role cannot filter pruned images, so ALL unused images will be removed.

### `unifi_network_application_device_communication_expose`

| Type | Default |
|------|---------|
| bool | `true`  |

Expose device communication port.

Required for device communication.

### `unifi_network_application_device_communication_port`

| Type | Default |
|------|---------|
| int  | `8080`  |

Device communication port.

### `unifi_network_application_web_admin_expose`

| Type | Default |
|------|---------|
| bool | `true`  |

Expose web admin port.

### `unifi_network_application_web_admin_port`

| Type | Default |
|------|---------|
| int  | `8443`  |

Web admin port.

### `unifi_network_application_stun_expose`

| Type | Default |
|------|---------|
| bool | `true`  |

Expose STUN port.

### `unifi_network_application_stun_port`

| Type | Default |
|------|---------|
| int  | `3478`  |

STUN port.

### `unifi_network_application_device_discovery_expose`

| Type | Default |
|------|---------|
| bool | `true`  |

Expose device discovery port.

Required for UniFi device discovery.

### `unifi_network_application_device_discovery_port`

| Type | Default |
|------|---------|
| int  | `10001` |

Device discovery port.

### `unifi_network_application_l2_discovery_expose`

| Type | Default |
|------|---------|
| bool | `false` |

Expose L2 discovery port.

Optional, but required for 'Make controller discoverable on L2 network' option.

### `unifi_network_application_l2_discovery_port`

| Type | Default |
|------|---------|
| int  | `1900`  |

L2 discovery port.

### `unifi_network_application_guest_portal_redirect_http_expose`

| Type | Default |
|------|---------|
| bool | `false` |

Expose HTTP guest portal redirect port.

Optional, but required for UniFi guest portal HTTP redirection.

### `unifi_network_application_guest_portal_redirect_http_port`

| Type | Default |
|------|---------|
| int  | `8880`  |

HTTP guest portal redirect port.

### `unifi_network_application_guest_portal_redirect_https_expose`

| Type | Default |
|------|---------|
| bool | `false` |

Expose HTTPS guest portal redirect port.

Optional, but required for UniFi guest portal HTTPS redirection.

### `unifi_network_application_guest_portal_redirect_https_port`

| Type | Default |
|------|---------|
| int  | `8843`  |

HTTPS guest portal redirect port.

### `unifi_network_application_mobile_speedtest_expose`

| Type | Default |
|------|---------|
| bool | `false` |

Expose mobile throughput test port.

Optional, but required for UniFi mobile throughput test.

### `unifi_network_application_mobile_speedtest_port`

| Type | Default |
|------|---------|
| int  | `6789`  |

Mobile throughput test port.

### `unifi_network_application_remote_syslog_expose`

| Type | Default |
|------|---------|
| bool | `false` |

Expose remote syslog port.

Optional, but required if you wish to use remote syslog.

### `unifi_network_application_remote_syslog_port`

| Type | Default |
|------|---------|
| int  | `5514`  |

Remote syslog port.

## Example Playbooks

**Bare Minimum:**

```yml
---
- name: Deploy UniFi Network Application
  hosts: server
  roles:
    - role: tigattack.unifi_network_application
      vars:
        unifi_network_application_mongo_password: _!CHANGEME!_
        unifi_network_application_mongo_root_password: _!CHANGEME!_
```

## License

MIT

[build_badge]:  https://img.shields.io/github/actions/workflow/status/tigattack/ansible-role-unifi-network-application/test.yml?branch=main&label=Lint%20%26%20Test
[build_link]:   https://github.com/tigattack/ansible-role-unifi-network-application/actions?query=workflow:Test
[galaxy_badge]: https://img.shields.io/ansible/role/d/tigattack/unifi_network_application
[galaxy_link]:  https://galaxy.ansible.com/ui/standalone/roles/tigattack/unifi_network_application/
