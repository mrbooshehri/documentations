# Common ```pacman```

## Issue
```
error: failed to synchronize all databases (unable to lock database)
```

## Solution

```bash
sudo rm /var/lib/pacman/db.lck
```
