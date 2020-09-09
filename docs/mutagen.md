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
# Create a sync session named "test" between the local path ~/mutagen_test and a 
# SSH-accessible endpoint. 
mutagen sync create --name=test ~/mutagen_test john@:~/mutagen_test
``` 

For SSH-accessible endpoints,Mutagen uses OpenSSH under the hood, so all of your settings, keys and
aliases will automatically available.

## Session identification
Once session is created, each Mutagen session can be addressed in three different ways: by name,
by a label, or by session identifier. Labels are optional key-value pairs that can be attached to
sessions and queried to perform more complex session selection. Sessions identifiers are unique 
strings that Mutagen generates automatically, allowing for unambiguous session specification.

## Listings sessions
```shell script
$ mutagen sync list
Name: test
Identifier: sync_1MugMKr3HOZxR7Z768KCslzSmz4RHrU2z5TsjWODbFf
Labels: None
Alpha:
        URL: /Users/abrarkhan/mutagen_test
        Connection state: Connected
Beta:
        URL: john@192.168.8.101:~/mutagen_test
        Connection state: Connected
Status: Watching for changes
```

## Monitoring a session
The monitor command shows live monitoring info for a session

```shell script
$ mutagen sync monitor test
Name: test
Identifier: sync_1MugMKr3HOZxR7Z768KCslzSmz4RHrU2z5TsjWODbFf
Labels: None
Alpha: /Users/abrarkhan/mutagen_test
Beta: john@192.168.8.101:~/mutagen_test
Status: Staging files on beta: 75% (8942/11782)/Watching for changes
``` 

## Pausing Sessions
Synchronization can be temorarily halted for a session using the `pause` command

```shell script
$ mutagen sync pause test 
``` 

## Resuming sessions
Sessions can be resumed using `resume` command

```shell script
mutagen sync resume test
``` 

## Flushing sessions
Synchronization cycles can be manually triggered for synchronization sessions

```shell script
mutagen sync flush test
```

## Resetting sessions
Synchronization sessions can have their histories cleared(causing them to behave like newly created
sessions with the same configuration).

```shell script
mutagen sync reset test
```

## Terminating sessions
Synchronization can be permanently halted(and the session deleted) 

```shell script
mutagen sync terminate test
```