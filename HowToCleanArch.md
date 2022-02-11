# How to keep an Arch system clean and light

## Pacman cache

Pacman cached packages are under ```/var/cache/pacman/pkg```. There are
several ways to remove them which are listed below:

1. Manually
	* ```rm /var/cache/pacman/pkg/*```
1. Using ```pacman```
	1. Remove caches of not installed packages
		* ```sudo pacman -Sc```
	1. Remove all caches 
		*	```sudo pacman -Scc```
1. Using ```paccache``` - (**Note:** ```pacman-contrib``` needed)
	1. Manually
		* To see which packages going to remove run ```paccache -d```
		* To remove run ```paccache -r```
	1. Using ```systemd```
		* Add following lies to ```paccache.time``` under
			```/etc/systemd/system```

			```
			[Unit]
			Descirption=Clean-up old packman pkg

			[Timer]
			OnCalendar=monthly
			Persistent=true

			[Install]
			WantedBy=multi-user.target
			```

			* Enable the daemon ```sudo systemctl enable paccache.timer```
		1. Using Hooks
			* Add folloing line to ```paccache.hook``` under
				```/usr/share/libalpm/hooks```

				```
				[Trigger]
				Operation = Upgrade
				Operation = Install
				Operation = Remove
				Type = Package
				Target = *

				[Action]
				Description = Cleaning pacman cache with paccache.
				When = PostTransaction
				Exec = /usr/bin/paccache -r
				```

## Remove unused packages

1. List of orphan packages ```sudo pacman -Qtdq```
1. Remove orphan packages ```sudo pacman -Rns $(pacman -Qtdq)```

## User caches and unsed configs
1. Clear caches in home directory 
	1. See the size ```du -sh ~/.cache```
	1. Remove them ```rm -rf ~/.cache/*```
1. Remove unsued config files from ```~/.config```

## Remove duplicates, empty files/directories, and broken symlinks
1. Using this program ```rmlint```
	* Install it ```sudo pacman -S rmlint```
	* Pass any directory you want to check to it
		```rm lint /home/$USER```
	* Run the script made by ```rmlit``` which is in the same directory
		you passed to it.

## Find the largest files and directories
You can use built-in tools or install ```ncdu```
