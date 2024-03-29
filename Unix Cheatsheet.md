# Unix Cheatsheet

## Files
*List Directory* -`ls [-l (formatted listing)] [-a (hidden files)]`

*Move Directory* - `cd <path_to_dir>`

*Renaming (or moving) a file* - `mv <path_to_source> <destination>`

*Copying a file* - `cp <path_to_source> <destination>`

*Symbolic Link* - `ln -s <source (files to link to)> <destination (new dir these files will be accessible from)>`

## Applications
*Install a program* - `sudo apt-get install <program name>`

## Running programs

*Keep a command running after closing terminal session* - `nohup <command to run> <and any arguments>`



## Putting it all together in a .sh file
`#!/bin/bash` - Run the script with bash, rather than another shell.

`sudo apt-get update` - Updates the package lists for upgrades for packages that need upgrading, as well as new packages that have just come to the repositories.

`sudo apt-get -y` - Runs apt-get with an automatic "yes" to all prompts.

`sudo apt-get -qq` - Runs with a flag to apt-get to make it less noisy (no output except for errors).

`sudo. apt-get -fix-missing` - Ignore missing packages; If packages cannot be retrieved or fail the integrity check after retrieval (corrupted package files), hold back those packages and handle the result.

```
#!/bin/bash

sudo apt-get update
sudo apt-get install -y -qq -fix-missing install <package 1> <package 2> <package 3> ... <package n>
```


## Working with SFTP
### Connecting
`sftp <username>@<domain>` - This will then ask for your password. Domain can be a web domain, such as `@example.com`, or an IP address like `x.xxx.xxx.xxx`.

### SFTP commands
`get <remote_path> <local_path>` - This will retrieve a file from <remote_path> on the remote machine, and save it to <local_path> on the local machine.

`put <local_path> <remote_path>` - This will put a file from <local_path> on the local machine to <remote_path> on the remote machine.
