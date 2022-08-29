# reverse-ssh

This document explains how to setup a reverse ssh


## What is a reverse ssh ?

### Desciption

The goal is to use one SSH connection in one way between two computers to be able to open a second one in the opposite direction inside the first one.

This make computers reachable from outside their (not public) network.

This require one other computer, this one accessible (i.e. public ip address etc...), that can be utilized by the previous ones to connect to.

### Example

There is two computer: "A" and "B".

Computer "A" has a public ip address and can be reached from the Internet.

Computer "B" can reach the computer "A".

Computer "A" can not reach computer "B" by any mean.


Computer "B" will open a SSH connection to the computer "A".

Inside this connection, computer "B" will redirect its own SSH port to one of the computer "A" port (exposing it publicly or keeping it internal).

Computer "A" is now able to connect to computer "B" through this _tunnel_.


## Commands

### Preparation (computer A and B)

```shell
# Make sure ssh server is active
systemctl enable --now sshd
```

Make sure any firewall running allow SSH connections.

### Create computer B ssh-key (computer B)

```shell
# Generate a SSH key
ssh-keygen

# Copy the public key to computer "A"
cat .ssh/id_rsa.pub
```

### Register computer B ssh-key onto computer A (computer A)

```shell
# Paste the content of the computer "B" public key here
$EDITOR .ssh/authorized_keys
```

### Attemp SSH connection from computer B to computer A (computer B)

```shell
# Try to connect to computer "A", this should not ask for any password as the key will be used
ssh my_user@my_domain.com
```

### Setup automatic SSH connection (computer B)

#### "user service"

This service will execute for your user (`whoami`)

```shell
# Put autossh.service here
mkdir -p .config/systemd/user
$EDITOR .config/systemd/user/autossh.service

# Enable and start this service
systemctl --user enable --now autossh

# Verify the status of the service
systemctl --user status autossh
```

#### system service (~root)

~ TODO

### Attempt reverse SSH connection from computer A to computer B (computer A)

```shell

# Now go to computer "B" commands

# Paste the content of the computer "B" public key here
$EDITOR .ssh/authorized_keys

# Now go back to computer "B" commands

# Try to connect to computer "B" using the tunnel, this will ask the password of computer "B" user
ssh -p 30000 my_user_B@localhost
```
