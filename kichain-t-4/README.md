# Relayer Setup for `kichain-t-4`

This guide intend to technical setup of relayer, if you don't know how relayer works please go to this [link](https://docs.cosmos.network/master/ibc/relayer.html)

In this relayer setup I am connecting between `kichain-t-4` and `autonomy` chain. 

>Note: those who don't have any connection to other testchains, you can use `autonomy` chain for test ibc transactions.

## Relayer Info 
I am using `go-relayer`, if you want you can use `typescript` or `rust` relayer.

### Installation
```bash
git clone https://github.com/cosmos/relayer.git
cd relayer 
git checkout v0.9.3
make install
```
### Setup
- Init the relayer
```bash
rly config init
```
Above command will create `.relayer` folder in home directory with default `config.yaml` file

- Add both chains configs using files from `/config` folder.
```bash
rly chains add -f config/kichain-t-4.json
rly chains add -f config/autonomy.json
```
- Check chains status 
```bash
rly chains list 
```
### Adding keys
>Note: you can use same menmonic pharse used in kichain,  for both kichain and autonomy chain.
- Adding ki testnet keys to relayer
```bash
rly keys restore kichain-t-4  testkey "mnemonic phrase"
```
- Adding autonomy keys to relayer
```bash
rly keys restore autonomy  testkey "mnemonic phrase"
```
### Query balance 
- To verify configuration properly you can use account query in both chains
```bash 
rly q bal kichain-t-4  testkey
rly q bal autonomy  testkey
```
### Init lightclients
```bash
rly light init kichain-t-4 -f 
rly light init autonomy -f
```
Now check the chain status
```bash 
rly chains list     
```
### Path generation
```bash
rly paths gen kichain-t-4 autonomy <path> 
```
### Link the both chains
```bash 
rly tx link <path> -d
```
This will create client, connection, channels between two chains.
### Start Relayer
```bash
rly start <path>
```

### IBC txs
from kichain to autonomy
```bash
rly tx  xfer kichain-t-4 autonomy 1utki <reciver-autonomy-address> --path kitoatn -d
```

from autonomy to kichains
```bash
rly tx xfer autonomy kichain-t-4  1aut tki1ghzd9t0zj23ssew7zv8sg97l9xakh8r8nxmr4l  --path kitoatn -d
```


## Autonomy details
### Chain info
```json
{
  "chain-id": "autonomy",
  "rpc-addr": "http://20.42.119.7:26657",
  "account-prefix": "autonomy",
  "gas-adjustment": 1.5,
  "gas-prices": "1aut",
  "trusting-period": "336h",
  "key": "testkey"
}
```
### Faucet
> you can use ki seed to recover autonomy key through relayer. So no need to use autonomy binary.

autonomy [explorer](https://explorer.autonomy.network/) 
```bash
curl --header "Content-Type: application/json"   --request POST   --data '{"denom":"aut","address":"<your autonomy address>"}'   http://20.42.119.7:8000/credit
```
- Request above url to get the autonomy `aut` tokens.


