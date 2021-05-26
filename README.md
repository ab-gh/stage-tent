# stage-tent

This bot was created for use on the Discord Tentpole server. It automates the creation, opening, and closing, of public Stage Channels, with associated text channels.

## Requirements

- [discord.py](https://pypi.org/project/discord.py/)
- [discord-py-slash-command](https://pypi.org/project/discord-py-slash-command/)

## Setup

The following constants should be changed:

- `GUILD_ID`: the ID of the guild where the bot is used
- `STAGE_CATEGORY`: the category where new stage sessions are created
- `ARCHIVE_CATEGORY`: the category where closed text channels are archived
- `STAGE_HOST`: the role of Stage Hosts
- `STAGE_OBSERVER`: the role of Stage Observers


The `ARCHIVE_CATEGORY` category should have the following permissions:
- `@everyone`: View Channel ❌
- `@moderator`: View Channel ✔️

## Usage

### `/session create <string>`

This command creates a new "Stage Session", with the given name. The name must be unique (i.e. a stage channel with name `<string>` must not already exist).

It requires the `@host` or `@observer` role to run

It will create a Stage Channel `<string>` and Text Channel `#<string>-text` with the following permissions:
- `@everyone`: View Channel ❌
- `@host`: View Channel ✔️, Manage Channel ✔️, Manage Permissions ✔️
- `@observer`: View Channel ✔️, Manage Channel ✔️, Manage Permissions ✔️
- Command Invoker (the user who ran the command): View Channel ✔️, Manage Channel ✔️, Manage Permission ✔️ (this final permission indicates **ownership of the Session**)
------
### `/session start 📢<stage>`

This command "opens" the given Stage Channel (i.e. makes it available for the public). It will reject any `📢<stage>` that is not a Stage Channel, but **will not check for ownership**. 

Anyone with the `@host` or `@observer` role can start a session.

It will edit the `📢<stage>` Stage Channel and associated `#<stage>-text` Text Channel with the following permission changes:
- `@everyone`: View Channel ✔️
------
### `/session stop 📢<stage>`

This command "closes" the given Stage Channel. It will reject any `📢<stage>` that is not a Stage Channel, but **will not check for ownership**.

Anyone with the `@host` or `@observer` role can stop a session.

It will delete the `📢<stage>` Stage Channel.

It will edit the `#<stage>-text` Text Channel with the following permission changes:
- Move to `ARCHIVE_CATEGORY` category
- Rename to `#<stage>-archive`
- Sync permissions to `ARCHIVE_CATEGORY` category
