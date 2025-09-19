<div align="center">

# IDCHAIN Relaychain Deployment Handbook 
![Version](https://img.shields.io/badge/version-v1.0.0-blue)
</div>


## Run A Validator Node
### Initial Set-up​
#### Requirements​

The most common way for a beginner to run a validator is on a cloud server running Linux. You may choose whatever VPS provider that you prefer. As OS it is best to use a recent Debian Linux. For this guide we will be using Ubuntu 24.04, but the instructions should be similar for other platforms.

#### Reference Hardware​

The transaction weights in Polkadot are benchmarked on reference hardware. We ran the benchmark on VM instances of two major cloud providers: Google Cloud Platform (GCP) and Amazon Web Services (AWS). To be specific, we used n2-standard-8 VM instance on GCP and c6i.4xlarge on AWS. It is recommended that the hardware used to run the validators at least matches the specs of the reference hardware in order to ensure they are able to process all blocks in time. If you use subpar hardware you will possibly run into performance issues, get less era points, and potentially even get slashed.
* CPU
    * x86-64 compatible;
    * Intel Ice Lake, or newer (Xeon or Core series); AMD Zen3, or newer (EPYC or Ryzen);
    * 8 physical cores @ 3.4GHz; starting with January 2025, the recommendation is to use a hardware with at least 8 physical cores, see referenda for more details about the rationale;
    * Simultaneous multithreading disabled (Hyper-Threading on Intel, SMT on AMD);
    * Prefer single-threaded performance over higher cores count. A comparison of single-threaded performance can be found here.
* Storage
    * An NVMe SSD of 1 TB (As it should be reasonably sized to deal with blockchain growth). An estimation of current chain snapshot sizes can be found here. In general, the latency is more important than the throughput.
* Memory
    * 32 GB DDR4 ECC.
* System
    * Linux Kernel 5.16 or newer.
* Network
    * The minimum symmetric networking speed is set to 500 Mbit/s (= 62.5 MB/s). This is required to support a large number of parachains and allow for proper congestion control in busy network situations.

The specs listed above are not a hard requirement to run a validator, but are considered best practice. Running a validator is a responsible task; using professional hardware is a must in any way.

#### Install and Configure Network Time Protocol Client​

NTP is a networking protocol designed to synchronize the clocks of computers over a network. NTP allows you to synchronize the clocks of all the systems within the network. Currently it is required that validators' local clocks stay reasonably in sync, so you should be running NTP or a similar service. You can check whether you have the NTP client by running:
```
timedatectl
``` 

> <font color="#f00">Caution</font>
> <font color="#f00">If you are using Ubuntu 18.04 or a newer version, NTP Client should be installed by default.</font>

If NTP is installed and running, you should see System clock synchronized: yes (or a similar message). If you do not see it, you can install it by executing:

```
sudo apt-get install ntp
```

ntpd will be started automatically after install. You can query ntpd for status information to verify that everything is working:
```
sudo ntpq -p
```
> <font color="#F7A004">Warning</font>
> <font color="#F7A004">Skipping this can result in the validator node missing block authorship opportunities. If the clock is out of sync (even by a small amount), the blocks the validator produces may not get accepted by the network.
</font>

#### Make Sure Landlock is Enabled​
>Landlock is a Linux security feature used in Polkadot:
> Landlock empowers any process, including unprivileged ones, to securely restrict themselves.

To make use of landlock, make sure you are on the reference kernel version or newer. Most Linux distributions should already have landlock enabled, but you can check by running the following as root:
```
sudo dmesg | grep landlock || sudo journalctl -kg landlock
```

If it is not enabled, please see the official docs ("Kernel support") if you would like to build Linux with landlock enabled.

### Running the Relay Chain Binaries
#### Prerequisites: Install Rust and Dependencies​
If you have never installed Rust, you should do this first.

```
sudo apt install --assume-yes git clang curl libssl-dev protobuf-compiler
```

Then, run this command to fetch the latest version of Rust and install it.

```
curl https://sh.rustup.rs -sSf | sh -s -- -y
```

Please check and make sure you are running Rust version **1.74.0**.
Check your Rust version by run the following command.
```
cargo --version
```

If your version not match, run the following command to downgrade.
```
rustup install 1.74.0
rustup default 1.74.0
```

Now check again for the cargo version.
```
cargo --version
```


> <font color="#1936C9">note</font>
> <font color="#1936C9">If you do not have "curl" installed, run:</font>
> `sudo apt install curl`

It will also be valuable to have "websocat" (Netcat, curl and socat for WebSockets) installed for RPC interactions. Installation instructions for various operating systems can be found [here](https://github.com/vi/websocat#installation).

To configure your shell, run the following command.
```
source $HOME/.cargo/env
```

Verify your installation.
```
rustc --version
```

Finally, run this command to install the necessary dependencies for compiling and running the Polkadot node software.
```
sudo apt install make clang pkg-config libssl-dev build-essential
```

> <font color="#1936C9">note</font>
> <font color="#1936C9">If you are using OSX and you have Homebrew installed, you can issue the following equivalent command INSTEAD of the previous one:</font>
`brew install cmake pkg-config openssl git llvm`

#### Building the Binaries
You can build the validator binaries from the fork of polkadot-sdk. For IDCHAIN specifically, we will use [mandala-polkadot-v1.7.2](https://git.247.or.id/munir/idchain-polkadot-sdk/-/tree/mandala-polkadot-v1.7.2) branch to build the validator binaries.

```
git clone -b mandala-polkadot-v1.7.2 https://git.247.or.id/munir/idchain-polkadot-sdk.git
```

Go to the root of the repo and make sure you're on the correct branch:
```
cd idchain-polkadot-sdk
git branch
```
Build the binary:
```
cargo build --release
```

> <font color="#F7A004">warning</font>
> <font color="#F7A004">Run the command above only once! Always check first whether the binaries are already available or not in `/target/release`. If they are available, you don't need to build them again.</font>

After compiling Relay binaries, you can verify the installation by running:
```
./target/release/polkadot --version
```

It should return something like this:

```
polkadot 1.7.2-b8f1cb0d367
```

Now build subkey binary for key generation tool.
```
cargo build --package subkey --release
```
Check subkey version by running:
```
./target/release/subkey --version
```
It should return something like this:
```
subkey 9.0.0
```

#### Generate a Sudo Key

You will need to also create a Sudo key, which will have Root privileges on the relay chain. This will also avoid using predefined accounts and minimizing attacks on the relay network.

:::warning
**Important**
On a testnet, if you have already generated a sudo key, it is not very important to create it again. You can reuse it if you somehow need to redeploy the chain.
:::

Use `subkey` command to generate the `sudo` key:
```
./target/release/subkey generate --scheme Sr25519 
```

The command displays output similar to the following:
```
Secret phrase:       fresh gauge diet release catch egg method such process ancient erupt account
  Network ID:        substrate
  Secret seed:       0xc8d50f6db37f82ed6b0b607e9bf0d8d3dbce139b388e3407cd27c090613d1d04
  Public key (hex):  0x30d44a91f42a34ebc7c10b214a7d3bd54c99275826ea31491c94eea579a3a35e
  Account ID:        0x30d44a91f42a34ebc7c10b214a7d3bd54c99275826ea31491c94eea579a3a35e
  Public key (SS58): 5DAjDg22ErYgwqSdHtpL8cLTf4Bm7hVqsfXqwzS11geN48dT
  SS58 Address:      5DAjDg22ErYgwqSdHtpL8cLTf4Bm7hVqsfXqwzS11geN48dT
```

Store the `Secret phrase` and `secret seed` in a secure place. Take note of the ss58 public key as you will need it later.

#### Create a Custom Chain Specification 

After you generate the keys to use with your blockchain, you are ready to create a custom chain specification using those key pairs then share your custom chain specification with other validators.

To enable others to participate in your blockchain network, ensure that they generate their own keys. After you collect the keys for network participants, you can create a custom chain specification to replace the local chain specification.

For simplicity, the custom chain specification you create in this tutorial is a modified version of the `rococo-local` chain specification that illustrates how to create a two-node network. You can follow the same steps to add more nodes to the network if you have the required keys.

##### Modify the Local Chain Specification

To create a new chain specification based on the local specification:

1. Make sure you're still in the root directory.
2. Export the `dev` chain specification to a file named `relaychain-plain-chainspec.json` by running the following command:
```
./target/release/polkadot build-spec --disable-default-bootnode --chain rococo-local > relaychain-plain-chainspec.json
```
3. Open the `relaychain-plain-chainspec.json` file in a text editor.
4. Modify the `name` field to identify this chain specification as a custom chain specification.
`"name": "IDCHAIN Relaychain"`
5. Modify the `id` field.
`"id": "idchain_relaychain"`
6. Modify the `protocolId` field.
`"protocolId": "idchain_relaychain"`
7. Modify the `sudo` field. Make sure that there is only one key value there.
```json
 "sudo": {
          "key": "5DAjDg22ErYgwqSdHtpL8cLTf4Bm7hVqsfXqwzS11geN48dT"
},
```
8. Add the `sudo` key inside the array in `patch.balances.balances` if you want to give it a predefined balance. It should looks like this:
```
    // outer array
    [
            ...other accounts,
            [
              "5DAjDg22ErYgwqSdHtpL8cLTf4Bm7hVqsfXqwzS11geN48dT",
              1000000000000000000000
            ],
    ],
```
if you want to pre-fund the account with 1000000 tokens (assuming 15 decimals).
9. Make sure that there are two arrays inside `session.keys` array, similar to this:
```json
"session": {
          "keys": [
            [
              "5GNJqTPyNqANBkUVMN1LPPrxXnFouWXoe2wNSmmEoLctxiZY",
              "5GNJqTPyNqANBkUVMN1LPPrxXnFouWXoe2wNSmmEoLctxiZY",
              {
                "authority_discovery": "5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY",
                "babe": "5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY",
                "beefy": "KW39r9CJjAVzmkf9zQ4YDb2hqfAVGdRqn53eRqyruqpxAP5YL",
                "grandpa": "5FA9nQDVg267DEd8m1ZypXLBnvN7SFxYwV7ndqSYGiN9TTpu",
                "para_assignment": "5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY",
                "para_validator": "5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY"
              }
            ],
            [
              "5HpG9w8EBLe5XCrbczpwq5TSXvedjrBGCwqxK1iQ7qUsSWFc",
              "5HpG9w8EBLe5XCrbczpwq5TSXvedjrBGCwqxK1iQ7qUsSWFc",
              {
                "authority_discovery": "5FHneW46xGXgs5mUiveU4sbTyGBzmstUspZC92UhjJM694ty",
                "babe": "5FHneW46xGXgs5mUiveU4sbTyGBzmstUspZC92UhjJM694ty",
                "beefy": "KWByAN7WfZABWS5AoWqxriRmF5f2jnDqy3rB5pfHLGkY93ibN",
                "grandpa": "5GoNkf6WdbxCFnPdAnYYQyCjAKPJgLNxXwPjwTh6DGg6gN3E",
                "para_assignment": "5FHneW46xGXgs5mUiveU4sbTyGBzmstUspZC92UhjJM694ty",
                "para_validator": "5FHneW46xGXgs5mUiveU4sbTyGBzmstUspZC92UhjJM694ty"
              }
            ]
          ]
        }
```
10. Save your changes and close the file.

#### Convert the Chain Specification to Raw Format

After you prepare a chain specification with the validator information, you must convert it into a raw specification format before it can be used. The raw chain specification includes the same information as the unconverted specification. However, the raw chain specification also contains encoded storage keys that the node uses to reference the data in its local storage. Distributing a raw chain specification ensures that each node stores the data using the proper storage keys.

To convert a chain specification to use the raw format:
1. Open a terminal shell on your computer.
2. Make sure you're still at the root directory.
3. Convert the `relaychain-plain-chainspec.json` chain specification to the raw format with the file name `relaychain-raw-chainspec.json` by running the following command:

```
./target/release/polkadot build-spec --chain=relaychain-plain-chainspec.json --raw --disable-default-bootnode > relaychain-raw-chainspec.json
```

#### Generate node key for validators
These node keys used as node identity for each node. Generate 2 node keys and write it down, you'll need these keys on the next step.

```
./target/release/subkey generate-node-key
```
The output similar as follows:
```
12D3KooWB3MysXVoHYT3hG4w9cSrNYT14PQGjS2he3FzDdJUyWKH
0307d238474f5ce4e48807b69db649c6b10a8dba06bd0d7b984fc93c19a357ea
```
The first line is node-identity and the second one is node-key.

#### Prepare to launch the private network

After you distribute the custom chain specification to all network participants, you're ready to launch Relay Chain. If you follow the steps in this tutorial, you'll be able to add multiple computers to your network.

To continue, verify the following:

- You have generated or collected the account keys for at least two authority accounts.
- You have updated your custom chain specification to include the keys for block production (`aura`) and block finalization (`grandpa`).
- You have converted your custom chain specification to raw format and distributed the raw chain specification to the nodes participating in the private network.

If you have completed these steps, you are ready to start the first node in the relay chain.

#### Start the First Node

You are responsible for starting the first node, called the **bootnode**. For now, you are going to run the node using `--alice` account.

Create directory tree on your home directory as follows:
```
mkdir -p ~/idchain-validator/node/bin ~/idchain-validator/validator
```
```
idchain-validator/
├── node/
│   └── bin/
└── validator/
```

* The `node` directory for storing config
* The `node/bin` directory for archiving the binary
* The `validator` directory for storing blockchain data

Now go to project directory and copy the `relaychain-raw-chainspec.json` file to `node` directory, and copy runtime binary to `/usr/local/bin`
```
cp relaychain-raw-chainspec.json ~/idchain-validator/node/
cp -r target/release/polkadot* ~/idchain-validator/node/bin/
cp target/release/subkey ~/idchain-validator/node/bin/
sudo cp -r ~/idchain-validator/node/bin/* /usr/local/bin/
```

Create systemd service by running this command:
```
sudo vi /etc/systemd/system/validator.service
```

Copy the following config, and adjust some values:
```
[Unit]
Description=IDChain Validator

[Service]
ExecStart=/usr/local/bin/polkadot \
  --base-path /home/**<user>**/idchain-validator/validator \
  --chain /home/**<user>**/idchain-validator/node/relaychain-raw-chainspec.json \
  --port 30333 \
  --rpc-port 9944 \
  --rpc-cors all \
  --prometheus-port 9615 \
  --prometheus-external \
  --force-authoring \
  --validator \
  --alice \
  --node-key **<node_key_validator_1>**

Restart=always
RestartSec=120

[Install]
WantedBy=multi-user.target
```

Save the file and reload the daemon using command:
```
sudo systemctl daemon-reload
```
> <font color="#F7A004">warning</font>
> <font color="#F7A004"> It is recommended to delay the restart of a node with **RestartSec** in the case of node crashes. It's possible that when a node crashes, consensus votes in GRANDPA aren't persisted to disk. In this case, there is potential to equivocate when immediately restarting. What can happen is the node will not recognize votes that didn't make it to disk, and will then cast conflicting votes. Delaying the restart will allow the network to progress past potentially conflicting votes, at which point other nodes will not accept them. </font>

Now start the first node with the following command: 
```
sudo systemctl start validator
```

#### View information about node operations

After you start the local node, information about running validator can be check using this command : 
```
sudo journalctl -fu validator
```

In terminal, verify that you see output similar to the following:

```
2021-11-03 15:32:14 Substrate Node
2021-11-03 15:32:14 ✌️  version 3.0.0-monthly-2021-09+1-bf52814-x86_64-macos
2021-11-03 15:32:14 ❤️  by Substrate DevHub <https://github.com/substrate-developer-hub>, 2017-2021
2021-11-03 15:32:14 📋 Chain specification: Relay Chain Testnet
2021-11-03 15:32:14 🏷 Node name: Validator-Node-1
2021-11-03 15:32:14 👤 Role: AUTHORITY
2021-11-03 15:32:14 💾 Database: RocksDb at /tmp/node01/chains/local_testnet/db
2021-11-03 15:32:14 ⛓  Native runtime: node-template-100 (node-template-1.tx1.au1)
2021-11-03 15:32:15 🔨 Initializing Genesis block/state (state: 0x2bde…8f66, header-hash: 0x6c78…37de)
2021-11-03 15:32:15 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
2021-11-03 15:32:15 ⏱  Loaded block-time = 6s from block 0x6c78abc724f83285d1487ddcb1f948a2773cb38219c4674f84c727833be737de
2021-11-03 15:32:15 Using default protocol ID "sup" because none is configured in the chain specs
2021-11-03 15:32:15 🏷 Local node identity is: 12D3KooWLmrYDLoNTyTYtRdDyZLWDe1paxzxTw5RgjmHLfzW96SX
2021-11-03 15:32:15 📦 Highest known block at #0
2021-11-03 15:32:15 〽️ Prometheus exporter started at 127.0.0.1:9615
2021-11-03 15:32:15 Listening for new connections on 127.0.0.1:9945.
2021-11-03 15:32:20 💤 Idle (0 peers), best: #0 (0x6c78…37de), finalized #0 (0x6c78…37de), ⬇ 0 ⬆ 0
```

Take note of the following information:

* The output indicates that the chain specification being used is the custom chain specification you created and specified using the `--chain` command-line option.
* The output indicates that the node is an authority because you started the node using the `--validator` command-line option.
* The output shows the genesis block being initialized with the block hash `(state: 0x2bde…8f66, header-hash: 0x6c78…37de)`.
* The output specifies the Local node identity for your node. In this example, the node identity is `12D3KooWLmrYDLoNTyTYtRdDyZLWDe1paxzxTw5RgjmHLfzW96SX`.
* The output specifies the IP address used for the node is the local host `127.0.0.1`.

These values are for this specific tutorial example. The values in your output will be specific to your node and you must provide the values for your node to other network participants to connect to the bootnode.

Now that you have successfully started a validator node using your own keys and taken note of the node identity, you can continue to the next step. Before you add your keys to the keystore, however, stop the node by pressing `Control-c`.

#### Running other validators to join
You can now allow other validators to join the network using the `--bootnodes` and `--validator` command-line options. You will be using `--bob` account for this node.

Make sure you're already create the following directory tree and copy the content for `~/idchain-validator/node/` from `validator-1` to this node.
```
mkdir -p ~/idchain-validator/node/bin ~/idchain-validator/validator
```
```
idchain-validator/
├── node/
│   └── bin/
└── validator/
```

Before start, try to execute the binary and make sure it has same version with `validator-1`.
```
~/idchain-validator/node/bin/polkadot --version
```
It should return something like this:
```
polkadot 1.7.2-b8f1cb0d367
```
If everything OK, copy the binary to `/usr/local/bin`
```
sudo cp -r ~/idchain-validator/node/bin/* /usr/local/bin/
```

:::info
**Important**
If you got any error or the binary can't run on this node, you have to build the binary from scratch on this node.
:::

Create systemd service by running this command:
```
sudo vi /etc/systemd/system/validator.service
```

Copy the following config, and adjust some values:
```
[Unit]
Description=IDChain Validator

[Service]
ExecStart=/usr/local/bin/polkadot \
  --base-path /home/**<user>**/idchain-validator/validator \
  --chain /home/**<user>**/idchain-validator/node/relaychain-raw-chainspec.json \
  --port 30333 \
  --rpc-port 9944 \
  --rpc-cors all \
  --prometheus-port 9615 \
  --prometheus-external \
  --force-authoring \
  --validator \
  --bob \
  --node-key **<node_key_validator_2>** \
  --bootnodes /ip4/**<ip_address_validator_1>**/tcp/30333/p2p/**<node_identity_validator_1>**

Restart=always
RestartSec=120

[Install]
WantedBy=multi-user.target
```

Save the file and reload the daemon using command:
```
sudo systemctl daemon-reload
```   

Now start the first node with the following command: 
```
sudo systemctl start validator
``` 
You can check that it's working with:

```
sudo systemctl status validator.service
```
To enable this to autostart on bootup run:
```
sudo systemctl enable validator.service
```
You can tail the logs with `journalctl` like so:

```
sudo journalctl -f -u validator
```

The `--chain` command-line option specifies the chain specification file to use. This file must be identical for all validators in the network. Be sure to set the correct information for the `--bootnodes` command-line option. In particular, be sure you have specified the local node identifier from the first node in the network. If you don't set the correct bootnode identifier, you see errors like this:

```
The bootnode you want to connect to at ... provided a different peer ID than the one you expect: ...
```

You should see that each node has one peer (`1 peers`), and they have produced a block proposal (`best: #2 (0xe111…c084)`). After a few seconds, you should see new blocks being finalized on both nodes.

### Accessing Node Using PolkadotJS
[PolkadotJS](https://polkadot.js.org/apps/) is a suite of web-based tools and applications for interacting with Polkadot and other Substrate-based blockchains. The link you've provided points to the PolkadotJS Apps interface, a powerful browser-based dashboard for managing accounts, exploring chains, submitting extrinsics, and more.

Now install Nginx for websocket RPC
1. Install Nginx using this command
```
sudo apt install nginx
```
2. Configure Nginx
Create Nginx site configuration :
```
sudo vi /etc/nginx/sites-available/<subdomain>.idchain.id
```
Copy this Nginx configuration example and adjust the subdomain name. 
```
server {
    server_name <subdomain>.idchain.id;

    location / {
        proxy_pass http://localhost:9944;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_set_header Host $host;
    }

    listen 80;
}
```
Enable config by creating link on site-enable directory:
```
sudo ln -s /etc/nginx/sites-available/<subdomain>.idchain.id /etc/nginx/sites-enabled/<subdomain>.idchain.id
```
3. Test Nginx and reload
Make sure the A record of this subdomain already pointed to the node IP.
```
sudo nginx -t
sudo systemctl reload nginx
```
4. Install `letsencrypt` SSL for this domain (optional). You can skip this step if using other SSL certificate.
```
sudo apt -y install certbot python3-certbot-nginx
```
Register subdomain for letsencrypt SSL using certbot
```
sudo certbot --nginx -d <subdomain>.idchain.id --register-unsafely-without-email
```
5. Access node via PolkadotJS
Make sure the URL of the node can access via browser. 
Open URL [https://polkadot.js.org/apps/?rpc=wss://<subdomain>.idchain.id#/explorer](https://polkadot.js.org/apps/?rpc=wss://<subdomain>.idchain.id#/explorer). Adjust `subdomain` with your subdomain name for this node.

6. Using sudo account
Install **polkadot{.js} extension** on your browser.
Add account by choose **Import account from pre-existing seed**.
Click **Next**, input **Description** of this key and set password for your wallet.
Click **Connect Accounts**, check your sudo wallet name, and click "Connect Account(s)" button.
Refresh the PolkadotJS page and make sure there's **Sudo** menu appear under **Developer** section.

### References
1. https://wiki.polkadot.network/docs/maintain-guides-how-to-validate-polkadot
---
## IDCHAIN Relaychain Chainspec Guide


### Top-Level Fields

- `name`: The name of the relaychain network. Example: `IDCHAIN Relaychain`.
- `id`: Unique identifier for the relaychain. Example: `idchain_relaychain`.
- `chainType`: Type of chain (e.g., `Local`, `Live`, `Development`).
- `bootNodes`: List of network boot nodes (empty here, meaning no default nodes).
- `telemetryEndpoints`: Telemetry server endpoints for monitoring (null = disabled).
- `protocolId`: Protocol identifier for network communication.
- `properties`: Chain properties (null here, so no custom properties defined).
- `forkBlocks`: List of block hashes for forks (null = none defined).
- `badBlocks`: List of block hashes considered invalid (null = none defined).
- `lightSyncState`: State for light client synchronization (null = not set).
- `codeSubstitutes`: Used for runtime upgrades (empty here).
- `genesis`: Initial state of the chain.

---

### Genesis Section

#### `runtimeGenesis`
Contains all initial runtime configuration for the relaychain, such as authorities, balances, governance, and other modules. Common fields include:

- `authorities`: List of initial block authoring accounts for consensus.
- `balances`: Initial account balances, usually as an array of `[address, amount]` pairs.
- `council`: Governance council members (addresses).
- `technicalCommittee`: Technical committee members (addresses).
- `session`: Session keys for validators/authorities, mapping accounts to consensus keys.
- `sudo`: Superuser (administrator) key:
	- `key`: SS58-encoded address with sudo privileges. This account can perform privileged operations like runtime upgrades and emergency actions. Protect this key and consider removing it for full decentralization.
- Other modules: You may find additional modules such as `democracy`, `treasury`, `vesting`, etc., each with their own configuration for governance, funds, and scheduling.

Refer to your chainspec for the exact modules and values present in your network.

For more details, see [Polkadot/Substrate chainspec documentation](https://docs.substrate.io/reference/chain-specs/).
