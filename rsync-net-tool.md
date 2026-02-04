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

## View rsync.net Snapshots

- `ssh de19@de19.rsync.net ls -asl .zfs/snapshot/`

Once you find the file(s) you want, you can simply use the `scp` or `rsync` commands to download the file(s).

## Groups

Sub accounts are members of the parent group (de19) and their own group. The parent account (de19) is also a member of each sub account's group. This cross-membership enables flexible permission control.

### Home Directory

The parent home directory is set to `0750` — sub accounts can read and traverse it but cannot write or delete files in it.

A sub-home-directory is created inside the parent account for each sub account. It is owned by the parent account with the sub account's group. This means the sub account has full access to their own home directory, but the parent account retains full ownership.

The sub-home-directory name matches the sub account's login ID. You can create an alternate home directory with a custom name, but the login ID will still be numeric. Contact rsync.net to update their records if you create a custom home directory.

Each sub account has its own `.ssh` directory to place SSH keys in.

### Permissions

- **View all ownerships and permissions**
  - `ssh de19@de19.rsync.net ls -asl`

Because you have the parent account, the parent group, and individual sub accounts, you have three levels of security abstraction:

1. **Owner only** — only the parent account can access
   - `ssh de19@de19.rsync.net mkdir private`
   - `ssh de19@de19.rsync.net chmod 0700 private`

2. **Owner and one sub account** — owned by the parent user with the sub account's group
   - `ssh de19@de19.rsync.net chmod 0770 some-directory`

3. **Owner and all sub accounts** — accessible to the parent and all sub account group members
   - `ssh de19@de19.rsync.net mkdir incoming`
   - `ssh de19@de19.rsync.net chmod 0770 incoming`

### Data Privacy

You may prevent sub accounts from accessing parent account data. The easiest way is to create a "private" folder, move all private data inside it, and remove group and other permissions:

- `ssh de19@de19.rsync.net chmod go-rwx path/to/private/file_or_folder`

### Administration

The following actions are available for sub accounts in the rsync.net Account Manager:

- Create/adjust sub account quotas
- View sub account data usage
- Enable/disable account locks
- Create/alter snapshot configuration
- Create/alter idle warning alerts

## References

- [Remote Linux commands](https://www.rsync.net/resources/howto/remote_commands.html)
- [rsync usage guide](https://www.rsync.net/resources/howto/rsync.html)

Tags: #role-linux-admin #tool-rsync-net
