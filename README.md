# reverse-ssh
This explain how to setup a reverse ssh


## What is a reverse ssh

The goal is to use one SSH connection to open a second one inside in the opposite direction.

This make computers reachable from outside their network.

This require one computer that can be reached by them.


## Example

### Situation

There is two computer: "A" and "B".

Computer "A" has a public ip address and can be reached from the Internet.

Computer "B" can reach the computer "A".

Computer "A" can not reach computer "B" by any mean.

### Steps

Computer "B" will open a SSH connection to the computer "A".

Inside this connection, computer "B" will redirect its own SSH port to one of the computer "A" port (exposing it publicly or keeping it internal).

Computer "A" is now able to connect to computer "B" through this _tunnel_.

## Commands

### Computer "A"

```shell
systemctl enable --now sshd  # make sure ssh is active

```

### Computer "B"

```shell
systemctl enable --now sshd  # make sure ssh is active

```
