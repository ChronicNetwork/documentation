# Sync using a snapshot file
This is a fast way to sync a node with `morocco-1` chain.
A snapshot is nothing more than someone sharing a copy of his/her blockchain's data folder in a compressed file.

Our current authorized providers are (in alphabetical order): 

* **TBD**:

* **TBD**:


(make sure to include them in your delegations ;)!)

The fastest way to sync a peer is using State Sync and is described [here as an alternative]().


For this step, its necesary that you have followed [this manual]() previously in order to install the `chtd` binary.

1. Ensure your installed binary is the lastest release:
    ```
    chtd version
    ```
    Output should be: `1.1.0`

    If you are using Cosmovisor, 
     ```
    cosmovisor run version
    ```
    Output should be: 
        ```
        cosmovisor run version
        11:11AM INF running app args=["version"] module=cosmovisor path=/home/raul/.cht/cosmovisor/upgrades/v1.1/bin/chtd
        --> .1.1.0
        ```
2. If you are a new user, you should  **Initialize the folders:** change **_Moniker_** by your validator name (use quotes for two or more separated words *"Terp Slurpin"*)
    ```
    chtd init Moniker --chain-id morocco-1 --overwrite
    ```
    This will create a `$HOME/.cht` folder
3. **Download the Genesis** `genesis.json` file
    ```
    cd $HOME
    curl -s https://raw.githubusercontent.com/ChronicNetwork/net/main/mainnet/v1.1/genesis.json > ~/.cht/config/genesis.json
    ```
   Ensure you have the correct file. Run the SHA256SUM test:
    ```
     sha256sum $HOME/.cht/config/genesis.json
     <output> 
    ```
4. **Add to _config.toml_ file: server SEEDs:**

    ```
    sed -E -i 's/seeds = \".*\"/seeds = \"\"/' $HOME/.cht/config/config.toml
    ```
5. You can **set the minimum gas prices** for transactions to be accepted into your node’s mempool. This sets a lower bound on gas prices, preventing spam.
    ``` 
    sed -E -i 's/minimum-gas-prices = \".*\"/minimum-gas-prices = \"0.001ucgas\"/' $HOME/.cht/config/app.toml
    ```

6. **Open the P2P port (26656 by default)**
    ```
    sudo ufw allow 26656
    ```
7. **Download the file** with block-data from one of our current authorized service providers:

Our current authorized providers are (in alphabetical order): 

* **Paranormal Brothers**:
https://bc.paranorm.pro/
* **Polkachu**:
https://polkachu.com/tendermint_snapshots/bitcanna


You should download a compressed file, unpack it and start the daemon.
Follow the instructions at the webpages of the snapshot providers.

This is the folder's tree (without Cosmosvisor installation). You should decompress it at `.cht/data/` 
```
.cht
├── config
└── data
    ├── application.db
    ├── blockstore.db
    ├── cs.wal
    ├── evidence.db
    ├── snapshots
    │   └── metadata.db
    ├── state.db
    └── tx_index.db

17 directories
```


8. If you have downloaded and decompressed the data-block you can **run a first time** to see if `chtd` continues to sync:

    `chtd start --log_level info`
    ```
    3:31PM INF Committed state appHash=77D16BED3F109A4A05A971C92602029569E049DFC1DC128CFF5CCAE3158F4B1B height=3886 module=state txs=0
    3:31PM INF Indexed block height=3886 module=txindex
    3:31PM INF minted coins from module account amount=1034628cgas from=mint module=x/bank
    3:31PM INF Executed block height=3887 invalidTxs=0 module=state validTxs=0
    3:31PM INF commit synced commit=436F6D6D697449447B5B38332031333820373720313731203135362032333220313431203435203137332037372031352031363020373120393720393520352031393020313836203733203131342034322031313620313230203536203338203230203337203437203231392032353220343920385D3A4632467D
    3:31PM INF Committed state appHash=538A4DAB9CE88D2DAD4D0FA047615F05BEBA49722A7478382614252FDBFC3108 height=3887 module=state txs=0
    ```

9. **Service creation**
**Ensure that you've stopped** the previous test with CTRL+C.
With all configurations ready, you can start your blockchain node with a single command (`chtd start`). In this tutorial, however, you will find a simple way to set up `systemd` to run the node daemon with auto-restart.

At this point you can create a simple CHTD service file, or you can configure Cosmovisor, Cosmovisor works like an upgrade "supervisor" that checks and applies the correct version of the software. As a result you can setup Cosmovisor as a replacement of the chtd daemon/command line utility and let Cosmovisor handle future upgrades.

So the next step is **(one of the following)**
* Skip Cosmovisor and continue with simple system service creation
* Go to [Cosmovisor](https://github.com/BitCannaGlobal/bcna/blob/main/1.2.setup-cosmovisor.md) guide and skip this step and the following steps of this guide.


Setup `chtd` systemd service (copy and paste all to create the file service):
```
    cd $HOME
    echo "[Unit]
    Description=Chronic Network Node
    After=network-online.target
    [Service]
    User=${USER}
    ExecStart=$(which chtd) start
    Restart=always
    RestartSec=3
    LimitNOFILE=4096
    [Install]
    WantedBy=multi-user.target
    " >chtd.service
```
    
Enable and activate the CHTD service.

```
    sudo mv chtd.service /lib/systemd/system/
    sudo systemctl enable chtd.service && sudo systemctl start chtd.service
```
Check the logs to see if it is working:
    ```
    sudo journalctl -u chtd -f
    ``` 
    
10. **Check the synchronisation:** If `catching_up = true` the node is syncing. Also you can compare your current block with the last synced block of another node, or at our [Explorer](https://explorer.bitcanna.io):
    ```
    curl -s localhost:26657/status  | jq .result.sync_info.catching_up
    #true output is syncing - false is synced

    curl -s localhost:26657/status | jq .result.sync_info.latest_block_height
    #this output is your last block synced

    curl -s "http://seed1.bitcanna.io:26657/status?"  | jq .result.sync_info.latest_block_height
    #this output the public node last block synced
    ```


###### tags: `doc` `github`