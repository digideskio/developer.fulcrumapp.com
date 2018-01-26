---
layout: default
section: desktop
order: 2
title: "Fulcrum Desktop Reference"
description: "Command reference for Fulcrum Desktop."
category: section
search: true
---

## Usage

{:.table.table-striped.event-table}
| OS | Command |
| - | - |
| macOS / Linux  | `fulcrum <cmd> [args]`  |
| Windows | `.\fulcrum.cmd <cmd> [args]` |

## Options

{:.table.table-striped.event-table}
| Option| Description | Type |
| - | - | - |
| `--version` |Show version number |[boolean]
| `--help` |Show help |[boolean]

## Commands

All command line arguments are also configurable via environment variables, prefixed with `FULCRUM_`. e.g. setting `--home-path` from an environment variable using `FULCRUM_HOME_PATH`. Explicit CLI arguments win. e.g. `fulcrum setup --home-path <path>` (to set the file system location for data, config, plugins, etc.); required for `setup`, `sync`, and `install-plugin` if not set as an environment variable.

### setup

Setup the local Fulcrum database.

Note: `setup` by default is run interactively on Linux/macOS, via prompts for email/password. Windows requires passing email/password parameters.

{:.table.table-striped.event-table}
| Option | Description | Required | Default |
| - | - | - | - |
| `--email` | email associated with your Fulcrum account | true | true|
| `--password` | password for your Fulcrum account | true | true |
| `--token <token>` | skip email/password and use an API token | false | false |

{:.table.table-striped.event-table}
| OS | Command |
| - | - |
| macOS / Linux  | `fulcrum setup`  |
| Windows | `.\fulcrum.cmd setup --email EMAIL --password SECRET` |

### sync

Sync an organization to the local database. Defaults to a one-time sync, but can continually sync (10 second intervals) using the `--forever` option.

{:.table.table-striped.event-table}
| Option | Description | Required | Default |
| - | - | - | - |
| `--org` | organization name | true | na|
| `--forever` | keep the sync running forever | false | false |
| `--clean` | start a clean sync, all data will be deleted before starting | false | false |
| `--after-sync-command <command>` | run an arbitrary command after each sync | false | false |
| `--no-progress` | disable progress logs, automatically disabled if stdout isn't a tty | false | false |
| `--simple-output` | replace emoji-based status indicators with text-based | false | false |
| `--no-colors` | disable console colors, automatically disabled for dumb terminals | false | false |
| `--form <id>` | filter by form ID, use multiple `--form` params for multiple forms | false | false |

{:.table.table-striped.event-table}
| OS | Command |
| - | - |
| macOS / Linux  | `fulcrum sync --org 'Organization Name'`  |
| Windows | `.\fulcrum.cmd sync --org "Organization Name"` |

_Windows seems to prefer double quotes with command parameters._

### reset

Reset an organization.

{:.table.table-striped.event-table}
| OS | Command |
| - | - |
| macOS / Linux  | `fulcrum reset --org 'Organization Name'`  |
| Windows | `.\fulcrum.cmd reset --org "Organization Name"` |

### install-plugin

Install a plugin.

{:.table.table-striped.event-table}
| Option | Description | Required | Default |
| - | - | - | - |
| `--name` | the plugin name | false | na|
| `--url` | the URL to a git repo | false | false |

{:.table.table-striped.event-table}
| OS | Command |
| - | - |
| macOS / Linux  | `fulcrum install-plugin --url https://github.com/fulcrumapp/fulcrum-desktop-postgres`  |
| Windows | `.\fulcrum.cmd install-plugin --url https://github.com/fulcrumapp/fulcrum-desktop-postgres` |

### create-plugin

Create a new plugin.

{:.table.table-striped.event-table}
| Option | Description | Required | Default |
| - | - | - | - |
| `--name` | the new plugin name | true | na|

{:.table.table-striped.event-table}
| OS | Command |
| - | - |
| macOS / Linux  | `fulcrum create-plugin --name 'MyPlugin'`  |
| Windows | `.\fulcrum.cmd create-plugin --name "MyPlugin"` |

### update-plugins

Update all plugins.

{:.table.table-striped.event-table}
| OS | Command |
| - | - |
| macOS / Linux  | `fulcrum update-plugins`  |
| Windows | `.\fulcrum.cmd update-plugins` |

### build-plugins

Build all plugins.

{:.table.table-striped.event-table}
| OS | Command |
| - | - |
| macOS / Linux  | `fulcrum build-plugins`  |
| Windows | `.\fulcrum.cmd build-plugins` |

### watch-plugins

Watch and recompile all plugins.

{:.table.table-striped.event-table}
| Option | Description | Required | Default |
| - | - | - | - |
| `--name` | plugin name to watch | false | na|

{:.table.table-striped.event-table}
| OS | Command |
| - | - |
| macOS / Linux  | `fulcrum build-plugins`  |
| Windows | `.\fulcrum.cmd build-plugins` |

### query

Run a query in the local database.

{:.table.table-striped.event-table}
| Option | Description | Required | Default |
| - | - | - | - |
| `--sql` | sql query | true | na|

{:.table.table-striped.event-table}
| OS | Command |
| - | - |
| macOS / Linux  | `fulcrum query --sql 'SELECT COUNT(*) FROM memberships'`  |
| Windows | `.\fulcrum.cmd query --sql "SELECT COUNT(*) FROM memberships"` |
