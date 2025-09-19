<div align="center">

# IDCHAIN Parachain Deployment Handbook 
![Version](https://img.shields.io/badge/version-v1.0.0-blue)
</div>


## Run A Collator Node
### Initial Set-up​
#### Requirements & Reference Hardware​
The prerequisites are similar as in Relaychain.
#### Install and Configure Network Time Protocol Client​

NTP is a networking protocol designed to synchronize the clocks of computers over a network. NTP allows you to synchronize the clocks of all the systems within the network. Currently it is required that collator's local clocks stay reasonably in sync, so you should be running NTP or a similar service. You can check whether you have the NTP client by running:
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
> <font color="#F7A004">Skipping this can result in the collator node missing block authorship opportunities. If the clock is out of sync (even by a small amount), the blocks the collator produces may not get accepted by the network.
</font>

#### Make Sure Landlock is Enabled​
>Landlock is a Linux security feature used in Polkadot:
> Landlock empowers any process, including unprivileged ones, to securely restrict themselves.

To make use of landlock, make sure you are on the reference kernel version or newer. Most Linux distributions should already have landlock enabled, but you can check by running the following as root:
```
sudo dmesg | grep landlock || sudo journalctl -kg landlock
```

If it is not enabled, please see the official docs ("Kernel support") if you would like to build Linux with landlock enabled.

### Running the Parachain Binaries
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
The code can be found on the private Gitlab Repo of IDCHAIN. For this instance, you'll use [feat/async-backing](https://git.247.or.id/pandi/idchain-testnet/-/tree/feat/async-backing) branch.

```
git clone -b feat/async-backing https://git.247.or.id/pandi/idchain-testnet.git
```

Go to the root of the repo and make sure you're on the correct branch:
```
cd idchain-testnet
git branch
```
Build the binary:
```
cargo build --release
```

The build process will take a while (30-80 minutes), depending on your machine.

After compiling parachain binaries, you can verify the installation by running:
```
./target/release/idchain-parachain --version
```

It should return something like this:

```
idchain-parachain 1.0.0-051b1691a43
```

#### Generate a Sudo Key, Collator Secret Keys and Node Key

You will need to also create a Sudo key, which will have Root privileges on the parachain.
Unlike Relay binary, IDCHAIN binary doesn't have key command. Instead, you can use the subkey, a CLI to generate secret keys for IDCHAIN collators. Each collator must have a unique secret key.

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
Secret phrase:       beach brass cushion parent fold choose hero report belt quantum athlete sentence
  Network ID:        substrate
  Secret seed:       0x2a765a2ce002d051b5101f7759c3fbe2fb591f7fb3c1634e7fb73e30588142f6
  Public key (hex):  0xd4720336b6f7b7a6e1c5bf95a02fb0e563c67e6e78591f57a536a3167776ed44
  Account ID:        0xd4720336b6f7b7a6e1c5bf95a02fb0e563c67e6e78591f57a536a3167776ed44
  Public key (SS58): 5GsFsrETgVaCqJHvtbaqK1zNnzttMBu9CEPATZKZ8mp6h6ED
  SS58 Address:      5GsFsrETgVaCqJHvtbaqK1zNnzttMBu9CEPATZKZ8mp6h6ED
```

Store the `Secret phrase` and `secret seed` in a secure place. Take note of the ss58 public key as you will need it later.

Now generate key for `collator-1` secret key.
```
./target/release/subkey generate --scheme Sr25519 
```
The command displays output similar to the following:
```
Secret phrase:       various ask phone drift save craft move trial knee laundry marble industry
  Network ID:        substrate
  Secret seed:       0x0e89af4790dfae46614482d924b372dd22bc15370991e5a2bc4ebf717464dafb
  Public key (hex):  0x7621383939295c851faecdeb72f39c6a0c1d4323a2c84d59af4b8a4236c9e50c
  Account ID:        0x7621383939295c851faecdeb72f39c6a0c1d4323a2c84d59af4b8a4236c9e50c
  Public key (SS58): 5EjbNkbW2xd4SDMTyV3s9enuVrnUmax4PwBAtF8qRQkiXQiS
  SS58 Address:      5EjbNkbW2xd4SDMTyV3s9enuVrnUmax4PwBAtF8qRQkiXQiS
```
As usual, store secrets in a safe place and take note of the ss58 public key.
Repeat the step above for `collator-2`.

For this example, the generated collator public keys are:
```
idchain-collator-1: 5EjbNkbW2xd4SDMTyV3s9enuVrnUmax4PwBAtF8qRQkiXQiS
idchain-collator-2: 5Hj4NzpWMY3MsQcEQUKmYFUcEnvE64kWNaovn2k1Fjyhcoe1
```

Also generate 2 node identity for each collator.
```
./target/release/subkey generate-node-key
```
The output similar as follows:
```
12D3KooWSW923RiUwKCoS6X8Xm34joGh3pDZUYvhqsHjwKVVHbFt
98f454a9d8c1eb28c9c017b2542a271dc479365533396016621b9c9e8fd22b9f
```
The first line is node-identity and the second one is node-key.
Repeat the step above for `collator-2`, so you've got total 4 node-identity.

#### Reserve a ParaId

Before reserving a ParaId for IDCHAIN on the relay, make sure you already have access to sudo mode on PolkadotJS.
    
Then you need to gain access to one of the validator nodes on the relay chain. Use `validator-1` for this example. After that, follow the steps:
    
1. Connect to `validator-1` via [Polkadot.js Apps](https://polkadot.js.org/apps/), adjust the RPC with your node subdomain.
2. Go to `Network` > `Parachains`
3. Go to `Parathreads` tab
4. Click the `+ ParaId` button
5. Choose your sudo wallet on `Reserve From` field
6. Take note of the value in `parachain id` field (normally it is `2000` if it is your first ParaId)
7. Click `Submit` and `Sign and Submit`
8. Go to `Network` > `Explorer`
9. You will see a `registrar.Reserved` event on `recent events` tab on the right. It means you have successfully reserved a ParaId.

#### Preparing Files to Become a Parachain
The steps below are basically the same as in validator node, with a slight modification.

1. Generate a plain chainspec:
```
./target/release/idchain-parachain build-spec --disable-default-bootnode --chain dev > parachain-plain-chainspec.json
```
2. Edit the chainspec:
    1. Modify the `name` field to identify this chain specification as a custom chain specification.
    `"name": "IDCHAIN Parachain"`
    2. Modify the `id` field.
    `"id": "idchain_parachain"`
    3. Modify the `chainType` field.
    `"chainType": "Local"`
    4. Modify the `protocolId` field.
    `"protocolId": "idchain_parachain"`
    5. Modify the `relay_chain` field to match the value of `protocolId` on `relaychain-raw-chainspec.json`.
    `"relay_chain": "idchain_relaychain"`,
    6. Change `para_id` and `parachainInfo.parachainId` to the previously registered paraId (in this case `2000`).
    7. Add two previously generated collator keys and sudo to "balances" fields.
    ```json
      "balances": {
          "balances": [
            [
              "5GsFsrETgVaCqJHvtbaqK1zNnzttMBu9CEPATZKZ8mp6h6ED",
              10000000000000000000000
            ],
            [
              "5EjbNkbW2xd4SDMTyV3s9enuVrnUmax4PwBAtF8qRQkiXQiS",
              10000000000000000000000
            ],
            [
              "5Hj4NzpWMY3MsQcEQUKmYFUcEnvE64kWNaovn2k1Fjyhcoe1",
              10000000000000000000000
            ],

    ```
    8. Add two previously generated collator keys to "invulnerables", "members" and "session.keys".
    ```json
    "collatorSelection":{
        "invulnerables": [
            "5EjbNkbW2xd4SDMTyV3s9enuVrnUmax4PwBAtF8qRQkiXQiS",
            "5Hj4NzpWMY3MsQcEQUKmYFUcEnvE64kWNaovn2k1Fjyhcoe1"
         ]
    }
    ...
    "council": {    
        "members": [
            "5EjbNkbW2xd4SDMTyV3s9enuVrnUmax4PwBAtF8qRQkiXQiS",
            "5Hj4NzpWMY3MsQcEQUKmYFUcEnvE64kWNaovn2k1Fjyhcoe1"
         ]
    }
    ...
    "session": {
          "keys": [
            [
              "5EjbNkbW2xd4SDMTyV3s9enuVrnUmax4PwBAtF8qRQkiXQiS",
              "5EjbNkbW2xd4SDMTyV3s9enuVrnUmax4PwBAtF8qRQkiXQiS",
              {
                "aura": "5EjbNkbW2xd4SDMTyV3s9enuVrnUmax4PwBAtF8qRQkiXQiS"
              }
            ],
            [
              "5Hj4NzpWMY3MsQcEQUKmYFUcEnvE64kWNaovn2k1Fjyhcoe1",
              "5Hj4NzpWMY3MsQcEQUKmYFUcEnvE64kWNaovn2k1Fjyhcoe1",
              {
                "aura": "5Hj4NzpWMY3MsQcEQUKmYFUcEnvE64kWNaovn2k1Fjyhcoe1"
              }
            ]
          ]
    }
    ...
    "technicalCommittee": {    
        "members": [
            "5EjbNkbW2xd4SDMTyV3s9enuVrnUmax4PwBAtF8qRQkiXQiS",
            "5Hj4NzpWMY3MsQcEQUKmYFUcEnvE64kWNaovn2k1Fjyhcoe1"
         ]
    }
   ```
   9. Change the "sudo.key" value with the previously generated Sudo key.
   ```json
    "sudo": {
          "key": "5GsFsrETgVaCqJHvtbaqK1zNnzttMBu9CEPATZKZ8mp6h6ED"
    },
    ```
3. Generate a raw chainspec:
```
./target/release/idchain-parachain build-spec --chain parachain-plain-chainspec.json --disable-default-bootnode --raw > parachain-raw-chainspec.json
```
4. Generate the genesis state:
```
./target/release/idchain-parachain export-genesis-head --chain parachain-plain-chainspec.json > genesis-state
```
5. Generate the validation code:
```
./target/release/idchain-parachain export-genesis-wasm --chain parachain-plain-chainspec.json > genesis-wasm
```

#### Registering IDCHAIN as Parachain
1. Go back to polkadot.js.org/apps on the relay chain side. Go to `Developer` > `Sudo`.
2. Select `parasSudoWrapper` and `sudoScheduleParaInitialize(id, genesis)`
enter the `reserved id (2000)` to `id` field
3. Enable file upload for both `genesisHead` and `validationCode` → because we will upload files for these.
4. Select `Yes` for `paraKind` → meaning when we register our parachain, it will be a parachain rather than a parathread.
5. Click on the `genesisHead` field then upload `genesis-state` file.
6. Click on the `validationCode` field then upload `genesis-wasm` file.
It should look like below:
![register-parachain](https://hackmd.io/_uploads/rym0LVuVkg.png)
7. Finalise with `Submit Sudo` and sign the transaction.

#### Running the first collator node (bootnode)
You'll need the file `relaychain-raw-chainspec.json` from validator node.

Create directory tree on your home directory as follows:
```
mkdir -p ~/idchain-collator/node/bin ~/idchain-collator/collator
```
```
idchain-collator/
├── node/
│   └── bin/
└── collator/
```

* The `node` directory for storing config
* The `node/bin` directory for archiving the binary
* The `collator` directory for storing blockchain data

Now go to project directory and copy the `parachain-raw-chainspec.json` file to `node` directory, and copy runtime binary to `/usr/local/bin`
```
cp parachain-raw-chainspec.json ~/idchain-collator/node/
cp -r target/release/idchain-parachain* ~/idchain-collator/node/bin/
sudo cp -r ~/idchain-collator/node/bin/* /usr/local/bin/
```

Also copy the `relaychain-raw-chainspec.json` file from validator to `node` directory.

Create systemd service by running this command:
```
sudo vi /etc/systemd/system/collator.service
```
Copy the following config, and adjust some values:
```
[Unit]
Description=IDChain Parachain Collator

[Service]
ExecStart=/usr/local/bin/idchain-parachain \
  --collator \
  --force-authoring \
  --chain /home/**<user>**/idchain-collator/node/parachain-raw-chainspec.json \
  --base-path /home/**<user>**/idchain-collator/collator \
  --port 40333 \
  --rpc-port 8844 \
  --rpc-cors all \
  --unsafe-rpc-external \
  --rpc-methods=Unsafe \
  --prometheus-port 9616 \
  --prometheus-external \
  --node-key **<node_key_collator_1-1>** \
  -- \
  --execution wasm \
  --chain /home/**<user>**/idchain-collator/node/relaychain-raw-chainspec.json \
  --port 30333 \
  --rpc-port 9944 \
  --rpc-cors all \
  --prometheus-port 9615 \
  --prometheus-external \
  --sync warp \
  --node-key **<node_key_collator_1-2>** \
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
sudo systemctl start collator
``` 
You can check that it's working with:

```
sudo systemctl status collator.service
```
To enable this to autostart on bootup run:
```
sudo systemctl enable collator.service
```
Now you have to inject key using this command:
```
curl -H "Content-Type: application/json" -d '{
  "jsonrpc": "2.0",
  "method": "author_insertKey",
  "params": [
    "aura",
    "**<seed_phrase_collator_1>**",
    "**<public_key(hex)_collator_1>**"
  ],
  "id": 1
}' http://localhost:8844
```
You can tail the logs with `journalctl` like so:

```
sudo journalctl -f -u collator
```

#### Running other collators to join
Make sure you're already create the following directory tree and copy the content for `~/idchain-collator/node/` from `collator-1` to this node.
```
mkdir -p ~/idchain-collator/node/bin ~/idchain-collator/collator
```
```
idchain-collator/
├── node/
│   └── bin/
└── collator/
```

Before start, try to execute the binary and make sure it has same version with `collator-1`.
```
~/idchain-collator/node/bin/polkadot --version
```
It should return something like this:
```
idchain-parachain 1.0.0-051b1691a43
```
If everything OK, copy the binary to `/usr/local/bin`
```
sudo cp -r ~/idchain-collator/node/bin/* /usr/local/bin/
```

:::info
**Important**
If you got any error or the binary can't run on this node, you have to build the binary from scratch on this node.
:::

Create systemd service by running this command:
```
sudo vi /etc/systemd/system/collator.service
```
Copy the following config and adjust some values:
```
[Unit]
Description=IDChain Parachain Collator

[Service]
ExecStart=/usr/local/bin/idchain-parachain \
  --collator \
  --force-authoring \
  --chain /home/**<user>**/idchain-collator/node/parachain-raw-chainspec.json \
  --base-path /home/**<user>**/idchain-collator/collator \
  --port 40333 \
  --rpc-port 8844 \
  --rpc-cors all \
  --unsafe-rpc-external \
  --rpc-methods=Unsafe \
  --prometheus-port 9616 \
  --prometheus-external \
  --node-key **<node_key_collator_2-1>** \
  --bootnodes /ip4/**<ip_address_collator_1>**/tcp/40333/p2p/**<node_identity_collator_1>** \
  -- \
  --execution wasm \
  --chain /home/**<user>**/idchain-collator/node/relaychain-raw-chainspec.json \
  --port 30333 \
  --rpc-port 9944 \
  --rpc-cors all \
  --prometheus-port 9615 \
  --prometheus-external \
  --sync warp \
  --node-key **<node_key_collator_2-2>** \
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
sudo systemctl start collator
``` 
You can check that it's working with:

```
sudo systemctl status collator.service
```
Now you have to inject key using this command:
```
curl -H "Content-Type: application/json" -d '{
  "jsonrpc": "2.0",
  "method": "author_insertKey",
  "params": [
    "aura",
    "**<seed_phrase_collator_2>**",
    "**<public_key(hex)_collator_2>**"
  ],
  "id": 1
}' http://localhost:8844
```
To enable this to autostart on bootup run:
```
sudo systemctl enable collator.service
```
You can tail the logs with `journalctl` like so:

```
sudo journalctl -f -u collator
```

Now you should see block production in your node terminal!
![node-success](https://hackmd.io/_uploads/rk53K4_V1l.jpg)

At the very beginning, once the node starts, you will see the message `Is collating: yes` on the logs:

```
Jan 30 10:46:08 parachain[447622]: 2025-01-30 10:46:08 Is collating: yes
Jan 30 10:46:14 parachain[447622]: 2025-01-30 10:46:14 [Relaychain] 🏷  Local node identity is: 12D3KooWAt5tpDcaopYb8AfEij3ZcV2wwXZCcaTnDgkXziYQD7aV
```

After a couple of seconds, the node should begin proposing and pre-seal a block to the network:

```
Jan 30 10:42:18 parachain[400695]: 2025-01-30 10:42:18 [Parachain] 🎁 Prepared block for proposing at 71 (1 ms) [hash: 0x45dd8e28d270f9c8e0bf31bef43c6f19468d8edc8001b5467c1ce45b7682a22a; parent_hash: 0xff96…fc40; extrinsics (2): [0x322b…0a7b, 0x7d2a…f95d]
Jan 30 10:42:18 parachain[400695]: 2025-01-30 10:42:18 [Parachain] 🔖 Pre-sealed block for proposal at 71. Hash now 0x5183fb0e251730e63e6925bb719e4775f2374e013eb576da940b4af795aacde8, previously 0x45dd8e28d270f9c8e0bf31bef43c6f19468d8edc8001b5467c1ce45b7682a22a.
```

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
        proxy_pass http://localhost:8844;
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
1. https://docs.kilt.io/docs/participate/staking/become_a_collator/setup-node
---
## IDCHAIN Parachain Chainspec Guide

### Top-Level Fields

- `name`: The name of the parachain network. Example: `IDCHAIN Parachain`.
- `id`: Unique identifier for the parachain. Example: `idchain_parachain`.
- `chainType`: Type of chain (e.g., `Local`, `Live`, `Development`).
- `bootNodes`: List of network boot nodes (empty here, meaning no default nodes).
- `telemetryEndpoints`: Telemetry server endpoints for monitoring (null = disabled).
- `protocolId`: Protocol identifier for network communication.
- `properties`: Chain properties:
  - `ss58Format`: Address format (38 for custom chains).
  - `tokenDecimals`: Number of decimals for the native token.
  - `tokenSymbol`: Symbol for the native token (e.g., `IDCHAIN`).
- `relay_chain`: Name of the relay chain this parachain connects to.
- `para_id`: Parachain ID assigned by the relay chain.
- `codeSubstitutes`: Used for runtime upgrades (empty here).
- `genesis`: Initial state of the chain.

---

### Genesis Section

#### `runtimeGenesis`
- `authorities`: List of initial block authoring accounts (empty here).
- `auraExt`: Aura consensus extension settings (empty object).
- `balances`: Initial account balances:
  - `balances`: Array of `[address, amount]` pairs. Each entry sets the starting balance for an account.
- `collatorSelection`: Collator (block producer) settings:
  - `candidacyBond`: Amount required to become a collator (0 here).
  - `desiredCandidates`: Number of desired collators (0 here).
  - `invulnerables`: List of invulnerable collator addresses.
- `council`: Governance council members (addresses).
- `democracy`: Democracy module settings (empty object).
- `didLookup`: DID (Decentralized Identifier) lookup links (empty array).
- `fungibles`: Fungible asset settings:
  - `accounts`: List of asset accounts (empty).
  - `assets`: List of assets (empty).
  - `metadata`: Asset metadata (empty).
- `parachainInfo`: Info about the parachain:
  - `parachainId`: Parachain ID (matches top-level `para_id`).
- `parachainSystem`: Parachain system settings (empty object).
- `polkadotXcm`: Cross-chain messaging settings:
  - `safeXcmVersion`: XCM protocol version (4).
- `session`: Session keys for validators/collators:
  - `keys`: Array of `[address, address, {aura: address}]` for session key mapping.
- `sudo`: Sudo (superuser) key:
  - `key`: Address with sudo privileges.
- `system`: System module settings (empty object).
- `technicalCommittee`: Technical committee members (addresses).
- `technicalMembership`: Technical membership members (empty array).
- `tipsMembership`: Tips membership members (empty array).
- `treasury`: Treasury module settings (empty object).
- `vesting`: Vesting schedules:
  - `vesting`: Array of vesting entries (empty).

---

### Example Address Explanation
- Addresses like `5DtE6cJDy8WjNZdfRWkvkmVyr2YepuCVHFwU4Gn2PsBnqTvf` are SS58-encoded public keys for accounts on the chain.
- Amounts like `10000000000000000000000` are in the smallest unit (based on `tokenDecimals`).

For more details, see [Polkadot/Substrate chainspec documentation](https://docs.substrate.io/reference/chain-specs/).
