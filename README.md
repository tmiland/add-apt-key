# POSIX Script for installing APT keys
## General help
This script will help with installing PGP keys for APT repositories.

This script supports up to 2 arguments:
  - 1st argument is input file. This can be either:
    - An URL - key will be downloaded into current path (using wget or curl)
    - A filename - reads an existing key in current path
    - A path and a filename - reads an existing key in given path
  - 2nd argument is key output path and output name. This can be either:
    - Only filename - output path is set in config, saved as given filename
    - A path and a filename - output path is given here, saved as given filename
    - Only a path (end with /) - output path is given here, filename is taken from existing key
    - Empty - output path is set in config, filename is taken from existing key

This script has a config file `/usr/local/etc/add-apt-key.conf`, where the following variables can be set:
  - `keypath`   : path to store converted key - default is `/usr/share/keyrings`
  - `verbosity` : if set to Yes - displays extra output
  - `removetmp` : if set to Yes - remove input (non-converted) file

Example 1: (`PWD=/root`)

    sudo add-apt-key https://mariadb.org/mariadb_release_signing_key.asc /usr/local/share/keyrings/

Will download key in `/root`, convert it and store as `/usr/local/share/keyrings/mariadb_release_signing_key.gpg`

Example 2: (`PWD=/home/user`)

    sudo add-apt-key /root/mariadb_release_signing_key.asc /usr/local/share/keyrings/mariadbkey

Will use existing key in `/root`, convert it and store as `/usr/local/share/keyrings/mariadbkey.gpg`

Example 3: (`PWD=/home/user`)

    sudo add-apt-key mariadb_release_signing_key.asc mariadbkey

Will use existing key in `/home/user`, convert it and store as `/usr/share/keyrings/mariadbkey.gpg`

After installing the PGP key, it is also possible to add the key and repository to `/etc/apt/sources.list`
  - The choice to add this will be presented in the script as the first input option
  - If Yes is chosen, the repository string must be pasted as the second input option

This completes the key installation by adding the corresponding repository line to `/etc/apt/sources.list`

## Installation
Install by running the following commands:

    sudo curl -L https://raw.githubusercontent.com/ameinild/add-apt-key/master/add-apt-key -o /usr/local/bin/add-apt-key
    sudo curl -L https://raw.githubusercontent.com/ameinild/add-apt-key/master/add-apt-key.conf -o /usr/local/etc/add-apt-key.conf
    sudo chmod a+rx /usr/local/bin/add-apt-key
