```
echo "deb [signed-by=/usr/share/keyrings/wsl-translinux.gpg] https://wsl-translinux.arkane-systems.net/apt/ $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/wsl-translinux.list
curl -s https://wsl-translinux.arkane-systems.net/apt/wsl-translinux.gpg | gpg --dearmor | sudo tee /usr/share/keyrings/wsl-translinux.gpg >/dev/null
sudo apt update
sudo apt install -y systemd-genie
```
