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
### Apply keys to guest
copy your keys to your guest:
```bash
ssh-copy-id -i ~/.ssh/<rsa key>.pub <username>@<domain/ip>
```

**Error**: `Warning: the ECDSA host key for '<domain>' differs from the key for the IP address '<ip address>'`

**Solution**: `ssh-keygen -R <domain|IP>`


