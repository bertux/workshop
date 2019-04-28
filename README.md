# ETHSingapore Hackathon Raiden Network guide

This is the repo for the Raiden Network at the ETHSingapore hackathon.
Below you'll find a list of links and information needed to get going with Raiden for the hackathon.

### Prerequisites:
- Access to an Ethereum Kovan RPC endpoint
    - For example through [Infura](https://infura.io/login)
- A Kovan account and KETH. We've created a small tool that generates an account and sends KETH and tokens to it with just one simple command. Please see the [onboarding section](#on-boarding) below for instructions.
- The Raiden client itself. Please see the [getting Raiden](#getting-raiden) section below.
  - If you're on Windows we recommend that you install Raiden for Windows Subsystem for Linux (WSL)
- We have created [a gitter room](https://gitter.im/raiden-network/eth-singapore-hackathon) that you can use for asking questions or find out where you can find us if you need help or want to discuss something face-to-face.

### On-boarding:
We've created a simple script that generates a keystore / address and sends Kovan ETH and ETHSingaporeTokens to the generated address. Follow these simple steps:

#### macOS instructions
- Download the onboarder [macOS binary](https://raiden-nightlies.ams3.digitaloceanspaces.com/onboarder-macOS.zip):
```sh
curl -O https://raiden-nightlies.ams3.digitaloceanspaces.com/onboarder-macOS.zip
```
- Unzip the file:
```sh
unzip onboarder-macOS.zip
```
- And run it:
```sh
./onboarder
```

#### Linux instructions
- Download the onboarder [linux binary](https://raiden-nightlies.ams3.digitaloceanspaces.com/onboarder-linux.tar.gz):
```sh
curl -O https://raiden-nightlies.ams3.digitaloceanspaces.com/onboarder-linux.tar.gz
```
- Extract the file:
```sh
tar -xvzf onboarder-linux.tar.gz
```
- And run it:
```sh
./onboarder
```

### Getting Raiden
The fastest way to get up and running is to use the latest binary releases. Just follow the instructions below.

#### macOS instructions
- Download the [latest released macOS binary](https://github.com/raiden-network/raiden/releases/download/v0.100.2/raiden-v0.100.2-macOS-x86_64.zip):
```sh
curl -LJO https://github.com/raiden-network/raiden/releases/download/v0.100.2/raiden-v0.100.2-macOS-x86_64.zip
```
- Unzip the file:
```sh
unzip raiden-v0.100.2-macOS-x86_64.zip
```

#### Linux instructions
- Download the [latest released linux binary](https://github.com/raiden-network/raiden/releases/download/v0.100.2/raiden-v0.100.2-linux-x86_64.tar.gz):
```sh
curl -LJO https://github.com/raiden-network/raiden/releases/download/v0.100.2/raiden-v0.100.2-linux-x86_64.tar.gz
```
- Extract the file:
```sh
tar xvzf raiden-v0.100.2-linux-x86_64.tar.gz
```

### Running Raiden:
Once Raiden is installed it's time to fire it up. This is done with the following command (Please make sure the replace `raiden-binary` with the actual binary you just created above):
```sh
./raiden-binary \
    --keystore-path keystore \
    --network-id kovan \
    --environment-type development \
    --eth-rpc-endpoint https://kovan.infura.io/v3/6c73b08f3539487bb6d00fdd052992a7
```

The node will ask you to accept the disclaimer and then ask you to choose which address you want to use. The list should only contain the one address the onboarder tool generated for you.

It will take a bit of time for the node to finish launching.
You'll see that it's ready once you see the message stating that the Rest-API has been started.

You can now access the WebUI at [http://localhost:5001/](http://localhost:5001).

#### Tell the rest

You should now have running Raiden node. From here you can join the ETHSingaporeTokens network. We recommend posting your address in the gitter channel if you want to try it out with someone else hacking on Raiden.
You can also check out how the network is growing by checking out the [Raiden Explorer](https://kovan.explorer.raiden.network/tokens/0x98a345f06e3A5DFe28EE0af38dd0780b4C0ed73B) for the ETHSingaporeToken.

### API commands:

#### Open channels
The first thing to do when Raiden is up and running is to open a channel with someone. You can do so by opening a channel with the node below, or to find another hacker building on Raiden and open a channel with him. For this you just have to replace the `partner_address` below with his/her address.

```sh
curl -i -X PUT http://localhost:5001/api/1/channels \
    -H 'Content-Type: application/json' --data-raw \
    '{"partner_address": "0x0bae0289AAA26845224F528F9B9DefE69e01606E", \
    "token_address": "0x98a345f06e3A5DFe28EE0af38dd0780b4C0ed73B", \
    "total_deposit": 10000000000000000000}'
```

#### Deposit
If you ever need to top up a channel, you can use the following command:
```sh
curl -i -X PATCH http://localhost:5001/api/1/channels/ \
0x98a345f06e3A5DFe28EE0af38dd0780b4C0ed73B/ADDRESS_OF_PARTNER \
-H 'Content-Type: application/json' \
--data-raw '{"total_deposit": 15000000000000000000}'
```

#### Make payments
To make payments, choose the address of the partner you've opened a channel with and do the following:
```sh
curl -i -X POST http://localhost:5001/api/1/payments/ \
0x98a345f06e3A5DFe28EE0af38dd0780b4C0ed73B/ADDRESS_OF_RECEIVER \
-H 'Content-Type: application/json' --data-raw '{"amount": 100000}'
```

Feel free to change the amounts of the payments.

### Other resources
- [API documentation](https://raiden-network.readthedocs.io/en/latest/rest_api.html)
- [Raiden installation instructions](https://raiden-network.readthedocs.io/en/latest/overview_and_guide.html#installation)
- [Getting Started with Raiden API](https://raiden-network.readthedocs.io/en/latest/api_walkthrough.html)
- [ETHSingaporeToken](https://kovan.etherscan.io/address/0x98a345f06e3A5DFe28EE0af38dd0780b4C0ed73B#code)
- [Hackathon Gitter Room](https://gitter.im/raiden-network/eth-singapore-hackathon)
