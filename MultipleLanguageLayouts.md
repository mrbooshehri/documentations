# Add multiple languages

Add the following lines to ```/etc/X11/xorg.conf.d/00-keyboard.conf```

```
# Read and parsed by systemd-localed. It's probably wise not to edit this file
# manually too freely.
Section "InputClass"
        Identifier "system-keyboard"
        MatchIsKeyboard "on"
        #Option "XkbLayout" "us"
        Option "XkbLayout" "us,ir"
        Option "XkbOptions" "grp:alt_shift_toggle"
	
EndSection
```
