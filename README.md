# sane openvpn3 client wrapper

Using the new openvpn3 client is pretty annoying, so here's a small wrapper that should cover basic usage in a sane way.

## Installation

Download the script and put it somewhere in your path.

## Usage

```
ovpn [COMMAND]

commands:
	session		session configuration
	config		client configuration
	help		show this message

alternatively use '--' to pass arguments directly to /usr/bin/openvpn3
```

Instead of using completely arcane commands this has `session` and `config`, import a new config doesn't add a new one
when it already exists and the same goes for sessions (who thought that was a good idea?).

So alltogether pretty simple now:

```
$ ovpn config import ~/my-config.vpn --name my-config
$ ovpn config list
$ ovpn session start my-config
$ ovpn session list
$ ovpn session stop my-config
```

Also added a little helper switch to session commands: `-a|--all` - this starts/stops all active sessions.

```
$ ovpn session start -a
$ ovpn session stop -a
```
