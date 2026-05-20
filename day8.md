# Day 8 - After getting initial access
## shadow hacking
- https://null-byte.wonderhowto.com/how-to/crack-shadow-hashes-after-getting-root-linux-system-0186386/
 
 
## password cracking (udvidet)
- https://hashcat.net/wiki/doku.php?id=example_hashes
 
- `hashcat -m 0 -a 0 hashes.txt dictionary.txt`

- `-m 0` specifies the hash type as MD5 (each hash type has a corresponding code, MD5's is 0). 
- `-a 0` specifies attack mode 0, which is a dictionary attack.

- Hvilke filer er spændende at læse når man først har adgang

- ` /etc/passwd`
- ` /etc/shadow`
- ` /etc/crontab`
- ` all homes directories`
- ` config files for services running e.g. php, mysql...`
- ` database files if any to access`
- ` SSH keys ~/.ssh/id_rsa`
- ` shell history ~/.bash_history`

## Fantastiske kommandoer efter initial access:

- `uname -a`: Provides information about the system (OS, kernel version, etc.).
- `cat /etc/issue`: Shows the OS distribution.
- `cat /etc/passwd`: Lists all users.
- `sudo -l`: Lists commands the current user can run as root/sudo.
- `id`: Shows user and group IDs for the current user.
- `crontab -l`: Lists cron jobs for the current user.
- `cat /etc/crontab`: Shows system cron jobs.
- `find / -perm -4000 2>/dev/null`: Finds files with the SUID bit set.
- `cat /etc/sudoers`: Shows sudoers file.

Dagens rum: ??

Feedback: ??
