---
layout: default
section: desktop
order: 1
title: "Fulcrum Desktop Introduction"
description: "Introduction to using the Fulcrum Desktop utility."
category: section
menu:

---

Fulcrum Desktop is an experimental command line tool for intelligently syncing your Fulcrum Organization data to an on-site database. The local database is a complete API representation with search indexes and query tables, making it ideal for integrating Fulcrum with data driven applications, such as Business Intelligence (BI) and Geographic Information Systems (GIS).

_**This application is still very much under active development and has been designed to support extensibility and customization via plugins, so expect changes, improvements, and additional functionality in the coming weeks and months. While we encourage developers to test out this experimental tool and provide feedback, it is not a first-class Fulcrum product and official support should not be expected.**_

## How it works

You first run the `setup` command to authenticate to your Fulcrum account and setup the local Fulcrum SQLite database. Next you can run the `sync` command to sync with your Fulcrum organization. This will build all the necessary database tables, indexes, views, etc. and sync down all of your Fulcrum resources, including forms, records, choice lists, classification sets, projects, members, and roles.

The internal _fulcrum.db_ SQLite database is not very user-friendly, so you will likely want to install one of the database plugins.

## Installation

Fulcrum Desktop has installers for Windows (32-bit and 64-bit), macOS and Linux, which are available from the [releases page on GitHub](https://github.com/fulcrumapp/fulcrum-desktop/releases).

### Linux (Ubuntu x64)

```sh
curl -o- -L https://raw.githubusercontent.com/fulcrumapp/fulcrum-desktop/master/install.sh | sudo bash
```

Command installation location: _/opt/Fulcrum/scripts/fulcrum_

Plugin installation directory: _/home/username/.fulcrum/plugins_

Internal SQLite database: _/home/username/.config/Fulcrum/data/fulcrum.db_

### macOS

Open the .dmg, drag the icon to _Applications_. That's it! While running, the program will auto-update if there's a new release available.

Command installation location: _/usr/local/bin/fulcrum -> /Applications/Fulcrum.app/Contents/scripts/fulcrum_

Plugin installation directory: _/Users/username/.fulcrum/plugins_

Internal SQLite database: _/Users/username/Library/Application Support/Fulcrum/data/fulcrum.db_

### Windows

Install from the Setup .exe and follow the instructions. This will create a shortcut icon on your desktop (and open up a GUI window - _which is not currently functional_ - you can close this window). Double-clicking the shortcut icon will open the GUI again; doing this also triggers the auto-updater. If there is an updated release available, it will be downloaded and installed.

Command installation location: _\Users\username\AppData\Local\Programs\Fulcrum\scripts\fulcrum.cmd_

Plugin installation directory: _\Users\username\\.fulcrum\plugins_

Internal SQLite database: _\Users\username\AppData\Local\Fulcrum\data\fulcrum.db_

## Getting started

After installing the core command line tools, you should authenticate with your Fulcrum account to setup your local database, sync your Organization's data down, install one of the [database plugins](/desktop/plugins/#database-plugins), and then setup auto-syncing. You can authenticate to Fulcrum via username/password or API token.

### Setup the local Fulcrum database

{:.table.table-striped.event-table}
| OS | Command |
| - | - |
| macOS / Linux (prompt) | `fulcrum setup` |
| macOS / Linux  | `fulcrum setup --email 'EMAIL' --password 'SECRET'` |
| Windows | `.\fulcrum.cmd setup --email "EMAIL" --password "SECRET"` |
| All | `fulcrum setup --token <token>` |

### Sync your Organization

{:.table.table-striped.event-table}
| OS | Command |
| - | - |
| macOS / Linux  | `fulcrum sync --org 'Organization Name'`  |
| Windows | `.\fulcrum.cmd sync --org "Organization Name"` |

_Note: if you have already installed the PostgreSQL plugin (below), you will also need to pass any parameters that your connection requires._  
_Example:_ `fulcrum sync --org 'Organization Name' --pg-user 'myuser' --pg-password 'mypassword' --pg-database 'mydatabase'`

### Install the PostgreSQL database plugin

By default, the PostgreSQL plugin expects a database named _fulcrumapp_ with the [PostGIS extension](http://www.postgis.net/) installed.

{:.table.table-striped.event-table}
| OS | Command |
| - | - |
| macOS / Linux  | `fulcrum install-plugin --name postgres`  |
| Windows | `.\fulcrum.cmd install-plugin --name postgres` |

### Setup continuous sync

Continually sync the database to pull down changes from Fulcrum.

{:.table.table-striped.event-table}
| OS | Command |
| - | - |
| macOS / Linux  | `fulcrum sync --org 'Organization Name' --pg-user 'myuser' --pg-password 'mypassword' --forever`  |
| Windows | `.\fulcrum.cmd sync --org "Organization Name" --pg-user "myuser" --pg-password "mypassword"  --forever` |

_Windows seems to prefer double quotes with command parameters._

## Next steps

Review the Fulcrum Desktop [command reference](/desktop/reference).
