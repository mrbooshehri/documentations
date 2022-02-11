# Backup and Restore system using rsync

1. Install rsync

```bash
sudo pacman -S rsync
```

1. Making new backup

	1. Run a test run (dry run), to make sure the backup works fine

```bash
sudo rsync -aAXv --delete --exclude=/dev/* --exclude=/proc/* \
--exclude=/sys/* --exclude=/tmp/* --exclude=/run/* --exclude=/mnt/* \
--exclude=/media/* --exclude="swapfile" --exclude="lost+found" \
--exclude=".cache" --exclude="Downloads" /source /destination
```

```markdown
Details:

-a, --archive				archive mode
-A, --acls					preserve ACL (Access Control List)
-X, --xattrs				preserve extended atributes
-v, --verbose				increase verbosity

--dry-run					simulate the bachup
--delete					delete extraneous files from dest dirs
--exclude=PATTERN			exclude files matching PATTERN
```

	1. Run previous command if nothing goes wrong

1. Restore data

````bash
sudo rsync -aAXv --delete --exclude="lost+found" /backup /system
````
