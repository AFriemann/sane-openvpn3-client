# sane openvpn3 client wrapper

Using the new openvpn3 client is pretty annoying, so here's a small wrapper that should cover basic usage in a sane way.

## Installation

Download the script and put it somewhere in your path. You'll probably need bash installed to run this, I didn't test it
on plain `sh`.

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
$ ovpn config import ~/my-config.vpn --name my-config
config my-config already exists
$ ovpn config list
❯ ovpn config list
Configuration path
Imported                        Last used                 Used
Name                                                      Owner
------------------------------------------------------------------------------
/net/openvpn/v3/configuration/122f8xefx832ax3xa299a35xda8fb8dad1ed
Mon Dec 12 14:45:57 2022        Mon Dec 12 16:40:02 2022  8
my-config                                                 me
------------------------------------------------------------------------------
$ ovpn session start my-config
$ ovpn session start my-config
session clarkgroup.openvpn.com already running:
        Path: /net/openvpn/v3/sessions/3293277a9210jnjka992929hhnajk423837a
     Created: Mon Dec 12 16:40:02 2022                  PID: 147527
       Owner: me                                     Device: tun0
 Config name: myorg.openvpn.com
Session name: de-fra.gw.openvpn.com
      Status: Connection, Client connected
$ ovpn session list
❯ ovpn session list
-----------------------------------------------------------------------------
        Path: /net/openvpn/v3/sessions/3293277a9210jnjka992929hhnajk423837a
     Created: Mon Dec 12 16:40:02 2022                  PID: 147527
       Owner: me                                     Device: tun0
 Config name: myorg.openvpn.com
Session name: de-fra.gw.openvpn.com
      Status: Connection, Client connected
-----------------------------------------------------------------------------
$ ovpn session stop my-config
Initiated session shutdown.

Connection statistics:
     BYTES_IN...................45760
     BYTES_OUT..................32292
     PACKETS_IN...................341
     PACKETS_OUT..................358
     TUN_BYTES_IN...............18971
     TUN_BYTES_OUT..............31997
     TUN_PACKETS_IN...............267
     TUN_PACKETS_OUT..............253

Cleaning up stale sessions - Found 0 open sessions to check
0 sessions removed
```

Also added a little helper switch to session commands: `-a|--all` - this starts/stops all active sessions.

```
$ ovpn session start -a
$ ovpn session stop -a
```

The command `session stop` also runs a cleanup of stale sessions because why would you want these around...

## Options

Set environment variables:

- `OVPN_PATH` defines the openvpn3 client path, by default retrieved using `which`
- `LOG_COMMANDS` write openvpn3 commands to stderr
