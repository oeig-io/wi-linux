# rsync.net Tool

The purpose of this tool is to provide off-server backup storage using native Linux commands.

This is important because rsync.net requires no proprietary client — standard SSH, scp, and rsync commands work directly.

## Common Commands

- **Change password**
  - `ssh -t de19@de19.rsync.net passwd`
- **List contents of root directory**
  - `ssh de19@de19.rsync.net ls`
- **Show tree of contents (all files)**
  - `ssh de19@de19.rsync.net tree`
- **Simple scp example**
  - `scp /path/to/some/file de19@de19.rsync.net:path/to/some/remote/directory/.`
- **Simple rsync example**
  - `rsync -avH /path/to/some/directory de19@de19.rsync.net:path/to/some/remote/directory`

## SSH Key Setup

Copy your local key to avoid entering a password with every command.

- **Create a local key** (do not overwrite an existing key)
  - `ssh-keygen`
- **First computer** (this overwrites any existing key in rsync.net)
  - `scp ~/.ssh/id_rsa.pub de19@de19.rsync.net:.ssh/authorized_keys`
- **Additional computers**
  - `cat ~/.ssh/id_rsa.pub | ssh de19@de19.rsync.net 'dd of=.ssh/authorized_keys oflag=append conv=notrunc'`

## References

- [Remote Linux commands](https://www.rsync.net/resources/howto/remote_commands.html)
- [rsync usage guide](https://www.rsync.net/resources/howto/rsync.html)

Tags: #role-linux-admin #tool-rsync-net
