# Install syncthing on debain/ubuntu
1. Install syncthing
```bash
sudo apt install curl apt-transport-https
curl -s https://syncthing.net/release-key.txt | sudo apt-key add -
echo "deb https://apt.syncthing.net/ syncthing release" | sudo tee /etc/apt/sources.list.d/syncthing.list
sudo apt update && apt install syncthing
```

2. Configure ```systemd``` unit
```bash
sudo vim /etc/systemd/system/syncthing@.service
```
Enter the following lines
```bash
[Unit]
Description=Syncthing - Open Source Continuous File Synchronization for %I
Documentation=man:syncthing(1)
After=network.target

[Service]
User=%i
ExecStart=/usr/bin/syncthing -no-browser -gui-address="0.0.0.0:8384" -no-restart -logflags=0
Restart=on-failure
SuccessExitStatus=3 4
RestartForceExitStatus=3 4

[Install]
WantedBy=multi-user.target
```
Reload systemd and start service
```bash
sudo systemctl daemon-reload
sudo systemctl start syncthing@$USER
sudo systemctl enable syncthing@$USER
```
