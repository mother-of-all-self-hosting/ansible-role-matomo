<!--
SPDX-FileCopyrightText: 2025 Oliver

SPDX-License-Identifier: GPL-3.0-or-later
-->

# Matomo Ansible Role

Matomo is a self-hosted, open source web analytics application. This role helps you to set up [Matomo's default docker image](https://hub.docker.com/_/matomo/), and configure it for Traefik and MariaDB.

This role *implicitly* depends on:

- [`com.devture.ansible.role.playbook_help`](https://github.com/devture/com.devture.ansible.role.playbook_help)
- [`com.devture.ansible.role.systemd_docker_base`](https://github.com/devture/com.devture.ansible.role.systemd_docker_base)

Check [defaults/main.yml](defaults/main.yml) for the full list of supported options.

For an Ansible playbook which integrates this role and makes it easier to use, see the [mash-playbook](https://github.com/mother-of-all-self-hosting/mash-playbook).

## Support

- Github issues: [acterglobal/ansible-role-matomo/issues](https://github.com/acterglobal/ansible-role-matomo/issues)
