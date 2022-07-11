# Cosmovisor setup guide for morocco-1

`Cosmovisor` is a small process manager for Cosmos SDK application binaries that monitors the governance module for incoming chain upgrade proposals. If it sees a proposal that gets approved, it stops the current binary, switch from the old binary to the new one, and finally restarts the node with the new binary.

This guide will explain how to install Cosmovisor and prepare for a future chain update. A full guide about Cosmovisor can be found [here](https://github.com/cosmos/cosmos-sdk/tree/master/cosmovisor).
 

## Step 1. Download Cosmovisor
You can build Cosmovisor from the source or download it from our GitHub repository. Detailed instruction to understand how it works [here](https://github.com/cosmos/cosmos-sdk/tree/master/cosmovisor#cosmosvisor).

### A) Download Cosmovisor

To download Cosmovisor without having to compile it yourself, run the following command:
```
cd ~
wget https://github.com/cosmos/cosmos-sdk/releases/download/cosmovisor%2Fv1.1.0/cosmovisor-v1.1.0-linux-amd64.tar.gz
```
Check sha256 sum
```
sha256sum cosmovisor-v1.1.0-linux-amd64.tar.gz
```
It must return: `9a6eb658404f28a3607a3d531821ca4a6501749aa483698847155f362e66859d`

Unzip the file, give it the correct permissions and move it to your machine's PATH
```
tar -xf cosmovisor-v1.1.0-linux-amd64.tar.gz
rm cosmovisor-v1.1.0-linux-amd64.tar.gz
chmod +x cosmovisor
sudo mv cosmovisor /usr/local/bin
```
### B) Build from source 

To compile Cosmovisor from the source, you need to have `go` installed on your system. A full guide to install `go` can be found [here](https://golangdocs.com/install-go-linux).

Try this to install Cosmovisor from the source by pulling the cosmos-sdk repository, switch to the correct version and build it:
```
git clone https://github.com/cosmos/cosmos-sdk.git
cd cosmos-sdk
git checkout cosmovisor/v1.1.0
make cosmovisor
```
The previous action builds Cosmovisor in the `/cosmovisor` directory, now let's move it to the system's PATH:
```
sudo mv cosmovisor/cosmovisor /usr/local/bin
```
## Step 2. Setup Cosmovisor

1) Create new directories
```
mkdir -p ${HOME}/.cht/cosmovisor/genesis/bin
mkdir -p ${HOME}/.cht/cosmovisor/upgrades/v1.1/bin
```
2) Download & copy the old version Chronic Network `v1.0`  binary to Cosmovisor's genesis folder.
```
cd ~
rm -f chtd #delete the binary if exist to avoid version mixings 
wget https://github.com/ChronicNetwork/cht/releases/download/v1.1/cht
cp ./chtd ${HOME}/.cht/cosmovisor/genesis/bin/
```
3) Download & copy the current version `v.1.1.0` to the upgrades folder.
This guide shows how to download the binary. If you want to build the binary from the source, detailed instructions can be found in the [README](https://github.com/ChronicNetwork/cht/blob/main/README.md) of our GitHub.

```
cd ~
rm -f chtd
wget -nc https://github.com/ChronicNetwork/cht/releases/download/v.1.1.0/cht
```
Check the sha256sum.
```
sha256sum ./chtd
```
It must return: ``

Verify that the version is:`.1.1.0`
```
chmod +x chtd
./chtd version
```
4) Move the newly built binary to the upgrades directory.
```
mv ./chtd ${HOME}/.cht/cosmovisor/upgrades/ruderalis/bin/
```
> If you build the binary from the code source move it to the same folder
4) Setup the current version link for Cosmovisor.
> Very important decision now. Depending on your choice you should sync the chain using a snapshot file/service or sync from the scratch (very slow process)
* If you want to sync from the scratch (from block 1) you should do:
    ```
    ln -s -T ${HOME}/.cht/cosmovisor/genesis ${HOME}/.cht/cosmovisor/current
    ```
* If you are going to sync using **(advanced users)** a snapshot file, StateSync or other you should point to the last version (v.1.3.1)
    ```
    ln -s -T ${HOME}/.cht/cosmovisor/genesis ${HOME}/.cht/cosmovisor/upgrades/ruderalis
    ```
5) To check if everything is OK, run:
```
ls .cht/cosmovisor/ -lh
```
The output should look like this:
```
total 8.0K
lrwxrwxrwx 1 user user   35 Jan 14 20:16 current -> /home/user/.cht/cosmovisor/genesis
drwxrwxr-x 3 user user 4.0K Jan 14 20:09 genesis
drwxrwxr-x 4 user user 4.0K Jan 14 20:15 upgrades
```


6. If you are a new user, you should  **Initialize the folders:** change **_Moniker_** by your validator name (use quotes for two or more separated words *"Terp Slurper"*)
```
chtd init Moniker --chain-id morocco-1 --overwrite
```
    This will create a `$HOME/.cht` folder
7. Download the Genesis `genesis.json` file
```
cd $HOME
curl -s https://raw.githubusercontent.com/ChronicNetwork/net/main/mainnet/v1.1/genesis.json > ~/.cht/config/genesis.json
```
   Ensure you have the correct file. Run the SHA256SUM test:
```
 sha256sum $HOME/.cht/config/genesis.json
 <output> 
```
8. Add to _config.toml_ file: server SEEDs:

```
sed -E -i 's/seeds = \".*\"/seeds = \"xxxx:26656\"/' $HOME/.cht/config/config.toml
```
9. You can **set the minimum gas prices** for transactions to be accepted into your node’s mempool. This sets a lower bound on gas prices, preventing spam.
``` 
sed -E -i 's/minimum-gas-prices = \".*\"/minimum-gas-prices = \"0.001ucgas\"/' $HOME/.cht/config/app.toml
```

10. Open the P2P port (26656 by default)
```
sudo ufw allow 26656
```
11) Create a systemd service for Cosmovisor. Simply copy and paste everything to create the service unit.
```
echo "[Unit]
Description=Cosmovisor Chronic Node Service
After=network-online.target
[Service]
User=${USER}
Environment=DAEMON_NAME=chtd
Environment=DAEMON_RESTART_AFTER_UPGRADE=true
Environment=DAEMON_HOME=${HOME}/.cht
Environment=UNSAFE_SKIP_BACKUP=true
Environment=DAEMON_LOG_BUFFER_SIZE=512
#Optional export DAEMON_DATA_BACKUP_DIR=${HOME}/your_chain_backup
ExecStart=$(which cosmovisor) run start
Restart=always
RestartSec=3
LimitNOFILE=4096
[Install]
WantedBy=multi-user.target
" >cosmovisor.service
```

12) Change **chtd** service for `cosmovisor` service. You can avoid the 3rd line if it is a clean installation and `chtd`service doesn't exist.
```
sudo mv cosmovisor.service /lib/systemd/system/
sudo systemctl daemon-reload
sudo systemctl stop chtd.service && sudo systemctl disable chtd.service 
sudo systemctl enable cosmovisor.service && sudo systemctl start cosmovisor.service
```

13) Check the logs to see if everything is OK. (ctrl + C to stop).
```
sudo journalctl -uf cosmovisor
```
> You can speed up the syncing using a StateSync Server or a snapshot file.

## Step 3. Finish the installation
If everything is right Cosmovisor will take control of the binaries. Instead of **chtd** you must use `cosmosvisor run` in your commands, for example: `cosmovisor run status`
To do this, make the following changes:

1) Add variables to the end of the **.profile** file.
```
nano $HOME/.profile
```
Add to the end of the file:
```
export DAEMON_NAME=chtd
export DAEMON_RESTART_AFTER_UPGRADE=true
export DAEMON_HOME=${HOME}/.cht
Environment=UNSAFE_SKIP_BACKUP=true
Environment=DAEMON_LOG_BUFFER_SIZE=512

#add this to continue to use chtd for commands, this is optional
PATH="${HOME}/.cht/cosmovisor/current/bin:$PATH" 
```

2) Reload the config of the **.profile** file.
```
source .profile
```
3) Now let's try Cosmovisor.

* Show Cosmovisor version: `cosmovisor run version`  Will be `v.1.2` before the upgrade and `v.1.3.1` after the upgrade
```
 cosmovisor run version
 ```
 ```
12:14PM INF running app args=["version"] module=cosmovisor path=/home/testnet/.cht/cosmovisor/genesis/bin/chtd
12:14PM ERR failed to read error="lstat /home/testnet/.cht/cosmovisor/current/upgrade-info.json: no such file or directory" filename=/home/testnet/.cht/cosmovisor/current/upgrade-info.json module=cosmovisor
1.2
```
* Show Chronic Network version: `chtd version` Must show the same version as above
* Show Cosmovisor sync info and status: `cosmovisor run status` 


## Reminder
In the future, you must use the `cosmovisor` command instead of the **chtd** command if you want to perform service related commands.

For example: 

* Start the service: `sudo service cosmovisor start`
* Stop the service: `sudo service cosmovisor stop`
* Restart the service: `sudo service cosmovisor restart`
* Check the logs: `sudo journalctl -u cosmovisor -f`