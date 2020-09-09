## An Overview
Mutagen is dev utility that lets you use your existing local tools to working remote machine like 
cloud servers and containers.It does this by providing high-performance file synchronization and 
flexible network forwarding, allowing you to develop applications in a way that looks local but runs
on remote hardware. It works directly between your local system, and the remote infrastructure that
you need to access.

## File synchronization
Mutagen's file synchronization is designed to facilitate real-time code editing with your existing
code editor orIDE, allowing you to quickly test code changes in a remote machine without having to 
re-deploy it. It has rsync-like performance,coupled with low-latency filesystem watching and the 
ability to operate in both unidirectional and bidirectional modes.It gives users granular control
over behaviours like conflict resolution, ignores, symbolic link handling, permission propagation, 
and more. It's also aggressively cross-platform,meaning that platform-specific are handled 
automatically and Windows-to-POSIX development is not a problem.

## Design
Mutagen is designed and operated around the concept of individual synchronization and forwarding sessions.
Each session operates between two endpoints. In the case of synchronization, these endpoints are 
file system locations, and in the case of forwarding, these endpoints are network endpoints.
Common usage scenarios include synchronizing source code from your laptop to remote server or container,
or forwarding requests from local TCP endpoints to remote web servers.

```bash
mutagen --help
mutagen <command> --help
```

## Mutagen Daemon 
The daemon is the core of the Mutagen architecture. It runs in the background as a per-user process,
hosting and managing synchronization sessions.

## Session management
Mutagen provides two sets of session management commands: one for synchronization(`mutagen sync`) 
and another for forwarding sessions (`mutagen forward`). They have approximately the same structure,
with only a few minor differences to account for their different purposes.

## Creating sessions
To create sync session, you use `mutagen sync create` command.

```shell script
# Create a sync session named "web-app-code" between the local path ~/project and a 
# SSH-accessible endpoint. 
mutagen sync create --name=web-app-code ~/project user@example.org:~/project
``` 

For SSH-accessible endpoints,Mutagen uses OpenSSH under the hood, so all of your settings, keys and
aliases will automatically available.