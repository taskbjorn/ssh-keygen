# SSH keypack

## Overview

Effortlessly generate SSH keys with a streamlined BASH script. This script
creates neatly organized archives with standardized file names, public/private
key pairs, and a matching checksum—perfect for secure and hassle-free storage.

> [!NOTE]
> **Why not use a Python script?**  
>   
> Of course SSH keypack could be much fancier, safer and modern as a Python
> script. But I wanted it to be extremely simple to deploy and use on any of my
> systems with minimal dependencies. It is just a hacky script for personal use!

## Dependencies

This script requires OpenSSH. For Debian-based distributions, you can install
OpenSSH with:

```ssh
sudo apt install --yes openssh-client
```
Create a symlink to your current user local binary folder to call `ssh-keypack`
from shell.

```shell
chmod +x ssh-keypack.sh
ln -s ssh-keygen ~/.local/bin/ssh-keypack
```

## Usage

To generate a key pack, run the script with the required command line arguments:

```shell
ssh-keypack <algorithm> <user> <host> <optional:git-username> <optional:git-repository>
```

All available positional command line arguments are listed below and can be
review from the command line via the `ssh-keypack --help` command:

| Positional argument | Required | Description |
| --- | :-: | --- |
| `algorithm` | Yes | Algorithm string passed to ssh-keygen. |
| `user` | Yes | Username associated with this SSH key pair. |
| `host` | Yes | Hostname associated with this SSH key pair. |
| `git-username` | No | Owner of the Git repository associated with this SSH key pair. |
| `git-repository` | No | Name of the Git repository associated with this SSH key pair. |

## Naming scheme

The fields marked with `<optional>` are meant for SSH keys to access specific
repositories and may be omitted.

* Comment schema for generic SSH key:
  `<algorithm>-key-<unix-seconds-timestamp>-<user>@<host>`
* Comment schema for Git repository SSH key:
  `<algorithm>-key-<unix-seconds-timestamp>-<user>@<host>:<git-user>+<git-repository>`

Filenames follow a scheme analogous to the SSH keys comment, but in the case of
Git repository paths replace the character `/` with `+` to comply with
filesystem naming restrictions.

The following extensions are used:

* `.ospubk`: public key in OpenSSH format
* `.ospk`: private key in OpenSSH format
* `.sha256`: SHA256 checkum of the private key

## Safety

Use `ssh-keypack` with care: it is a simple BASH script with no input validation
or safety checks of any kind.