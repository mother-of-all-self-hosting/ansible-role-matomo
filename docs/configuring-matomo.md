<!--
SPDX-FileCopyrightText: 2020 - 2024 MDAD project contributors
SPDX-FileCopyrightText: 2020 - 2024 Slavi Pantaleev
SPDX-FileCopyrightText: 2020 Aaron Raimist
SPDX-FileCopyrightText: 2020 Chris van Dijk
SPDX-FileCopyrightText: 2020 Dominik Zajac
SPDX-FileCopyrightText: 2020 Mickaël Cornière
SPDX-FileCopyrightText: 2022 François Darveau
SPDX-FileCopyrightText: 2022 Julian Foad
SPDX-FileCopyrightText: 2022 Warren Bailey
SPDX-FileCopyrightText: 2023 Antonis Christofides
SPDX-FileCopyrightText: 2023 Felix Stupp
SPDX-FileCopyrightText: 2023 Pierre 'McFly' Marty
SPDX-FileCopyrightText: 2024 - 2025 Suguru Hirahara

SPDX-License-Identifier: AGPL-3.0-or-later
-->

# Setting up LimeSurvey

This is an [Ansible](https://www.ansible.com/) role which installs [LimeSurvey](https://www.limesurvey.org) to run as a [Docker](https://www.docker.com/) container wrapped in a systemd service.

LimeSurvey is a feature-rich free software for web based forms and surveys, which supports extensive survey logic.

See the project's [documentation](https://www.limesurvey.org/manual/LimeSurvey_Manual) to learn what LimeSurvey does and why it might be useful to you.

>[!NOTE]
> Since the developer team behind LimeSurvey [declared](https://bugs.limesurvey.org/view.php?id=14606#c55854) that an official Docker image would not be available, this role uses the image provided by [ACSPRI](https://www.acspri.org.au/limesurvey), which is a LimeSurvey Authorised Partner for Australia (see [this list](https://www.limesurvey.com/index.php/hosting) of partners for confirmation).

## Prerequisites

To run a LimeSurvey instance it is necessary to prepare a [MySQL](https://www.mysql.com/) compatible database server.

If you are looking for an Ansible role for [MariaDB](https://mariadb.org/), you can check out [this role (ansible-role-mariadb)](https://github.com/mother-of-all-self-hosting/ansible-role-mariadb) maintained by the [Mother-of-All-Self-Hosting (MASH)](https://github.com/mother-of-all-self-hosting) team.

## Adjusting the playbook configuration

To enable LimeSurvey with this role, add the following configuration to your `vars.yml` file.

**Note**: the path should be something like `inventory/host_vars/mash.example.com/vars.yml` if you use the [MASH Ansible playbook](https://github.com/mother-of-all-self-hosting/mash-playbook).

```yaml
########################################################################
#                                                                      #
# limesurvey                                                           #
#                                                                      #
########################################################################

limesurvey_enabled: true

########################################################################
#                                                                      #
# /limesurvey                                                          #
#                                                                      #
########################################################################
```

### Set the hostname

To enable LimeSurvey you need to set the hostname as well. To do so, add the following configuration to your `vars.yml` file. Make sure to replace `example.com` with your own value.

```yaml
limesurvey_hostname: "example.com"
```

After adjusting the hostname, make sure to adjust your DNS records to point the domain to your server.

**Note**: hosting LimeSurvey under a subpath (by configuring the `limesurvey_path_prefix` variable) does not seem to be possible due to LimeSurvey's technical limitations.

### Set variables for connecting to a MySQL-compatible database server

To have the Limesurvey instance connect to your MySQL-compatible database server, add the following configuration to your `vars.yml` file.

```yaml
limesurvey_database_hostname: YOUR_MYSQL_SERVER_HOSTNAME_HERE
limesurvey_database_username: YOUR_MYSQL_SERVER_USERNAME_HERE
limesurvey_database_password: YOUR_MYSQL_SERVER_PASSWORD_HERE
limesurvey_database_name: YOUR_MYSQL_SERVER_DATABASE_NAME_HERE
```

### Set administrator's account details

You also need to specify administrator's account details by adding the following configuration to your `vars.yml` file:

```yaml
limesurvey_environment_variables_admin_user: LIMESURVEY_ADMIN_USERNAME_HERE
limesurvey_environment_variables_admin_password: LIMESURVEY_ADMIN_PASSWORD_HERE
limesurvey_environment_variables_admin_name: LIMESURVEY_ADMIN_NAME_HERE
limesurvey_environment_variables_admin_email: LIMESURVEY_ADMIN_EMAIL_ADDRESS_HERE
```

### Extending the configuration

There are some additional things you may wish to configure about the component.

Take a look at:

- [`defaults/main.yml`](../defaults/main.yml) for some variables that you can customize via your `vars.yml` file. You can override settings (even those that don't have dedicated playbook variables) using the `limesurvey_environment_variables_additional_variables` variable

See [the official documentation](https://hub.docker.com/r/acspri/limesurvey#how-to-use-this-image) for a complete list of LimeSurvey's config options that you could put in `limesurvey_environment_variables_additional_variables`.

## Installing

After configuring the playbook, run the installation command of your playbook as below:

```sh
ansible-playbook -i inventory/hosts setup.yml --tags=setup-all,start
```

If you use the MASH playbook, the shortcut commands with the [`just` program](https://github.com/mother-of-all-self-hosting/mash-playbook/blob/main/docs/just.md) are also available: `just install-all` or `just setup-all`

## Usage

After running the command for installation, LimeSurvey becomes available at the specified hostname like `https://example.com`.

To get started, open the URL `https://example.com/index.php/admin` with a web browser, and log in to the instance with the administrator account credentials.

## Troubleshooting

### Check the service's logs

You can find the logs in [systemd-journald](https://www.freedesktop.org/software/systemd/man/systemd-journald.service.html) by logging in to the server with SSH and running `journalctl -fu limesurvey` (or how you/your playbook named the service, e.g. `mash-limesurvey`).

#### Increase logging verbosity

If you want to increase the verbosity, add the following configuration to your `vars.yml` file:

```yaml
limesurvey_environment_variables_debug: 2
```
