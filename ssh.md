# SSH Setup

## References

[https://www.freecodecamp.org/news/the-ultimate-guide-to-ssh-setting-up-ssh-keys/](https://www.freecodecamp.org/news/the-ultimate-guide-to-ssh-setting-up-ssh-keys/)

## Host

### Manage Keys
1. `ssh-keygen` to create a new set of keys
2. Assign keys in a config file e.g. `~/.ssh/conf`
3. Only create new keys for new categories of servers, otherwise reuse the keys with conf settings.
```
Host <host alias>
  HostName <domain of host>
  User <username>
  IdentityFile </path/to/private_rsa_file>
  IdentityiesOnly yes
```
### Manage passwords
1. Make sure `ssh-agent` is running. execute  ``eval `ssh-agent -s` `` from the command line.
2. Add the key to the repository with `ssh-add`.
3. Add the following lines to the config file:
```
Host *
  AddKeysToAgent yes
```
