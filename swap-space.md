## Commands to create swap space space in Ununtu 18.04 LTS

sudo fallocate -l 1G /swapfile <br>
ls -lh /swapfile <br>
sudo chmod 600 /swapfile <br>
ls -lh /swapfile <br>
sudo mkswap /swapfile <br>
sudo swapon /swapfile <br>
sudo swapon --show <br>
<br>
<br>
<b>making swap permanent</b>
<br>
sudo cp /etc/fstab /etc/fstab.bak <br>
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab <br>
