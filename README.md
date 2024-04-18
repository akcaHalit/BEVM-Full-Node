! I've done it before, I'm just posting it again !
<h1 align="center">BEVM</h1>

> [Info](https://medium.com/@BTClayer2/announcing-incentivized-bevm-testnet-fullnode-program-31cbc047b950).

<h1 align="center">Hardware</h1>

```console
2 CPU 2 RAM
# There is no certainty about the disk, I used 80 GB disk to guarantee.
```

<h1 align="center">Kurulum</h1>

```console
# Server Update
sudo apt update
sudo apt upgrade
sudo apt install --assume-yes git clang curl libssl-dev llvm libudev-dev make protobuf-compiler
sudo ufw allow ssh; sudo ufw allow 30333; sudo ufw allow 20222; sudo ufw allow 30334

# Binary pulling
mkdir -p $HOME/.bevm
wget -O bevm https://github.com/btclayer2/BEVM/releases/download/testnet-v0.1.1/bevm-v0.1.1-ubuntu20.04 && chmod +x bevm
sudo cp bevm /usr/bin/

# Creating a service, customize your mmaddress.
sudo tee /etc/systemd/system/bevm.service > /dev/null << EOF
[Unit]
Description=BEVM
After=network-online.target
StartLimitIntervalSec=0
[Service]
User=root
Restart=always
RestartSec=3
ExecStart=/usr/bin/bevm --chain=testnet --name="yourmmaddress" --pruning=archive --telemetry-url "wss://telemetry.bevm.io/submit 0"
[Install]
WantedBy=multi-user.target
EOF

# Start the service
sudo systemctl daemon-reload
sudo systemctl enable bevm
sudo systemctl start bevm

# Be careful that the output control does not give an exit-code.
sudo journalctl -u bevm -f --no-hostname -o cat
```

> Telemetry Control
https://telemetry.bevm.io/#/0x41cfeafc7177775a0e838b3725a0178b89ebf5dde1b5f766becbf975a24e297b

