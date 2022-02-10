# Touchpad single tap click

Add the following lines to ```/etc/X11/xorg.conf.d/40-libinput.conf ```

```
Section "InputClass"
	Identifier "libinput touchpad catchall"
	MatchIsTouchpad "on"
	MatchDevicePath "/dev/input/event*"
	Option "Tapping" "True"
	Option "TappingDrag" "True"
	Option "AccelProfile" "linear"
	Option "DisableWhileTyping" "False"
	Driver "libinput"
EndSection
```
