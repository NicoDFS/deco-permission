# Permissioning Smart Contracts and Dapp

## Audit
Version 1 of these contracts was audited by a third party. Read the report [here](https://consensys.net/diligence/audits/2019/08/pegasys-permissioning/)

## License
The contents of this repository are Apache 2.0 licensed.
**Important:** The dependency chain for this Dapp includes [web3js](https://github.com/ethereum/web3.js/) which is LGPL licensed.

## Development
_Note: The build process for the Dapp is currently not supported on Windows._

### Initialize dependencies ###
Run `yarn install` to initialize project dependencies. This step is only required when setting up the project
for the first time.

### Linting
Linting is set up for contracts using `solium`, and for source files using `prettier`. To run linting over your code execute `yarn run lint`.

### Testing
`yarn test`

### Permissioning Management Dapp

The Dapp will facilitate managing permissioning rules and maintaining the list of admin accounts that can edit rules.


#### Compile and migrate the contracts (Development mode) ####
1. Delete your environment variables named `NODE_INGRESS_CONTRACT_ADDRESS`, `ACCOUNT_INGRESS_CONTRACT_ADDRESS`, `ACCOUNT_STORAGE_CONTRACT_ADDRESS`, `NODE_STORAGE_CONTRACT_ADDRESS` AND
`CHAIN_ID` - you might need to restart your terminal session to have your changes applied. If you are using a `.env` file, you can comment out the variables.
1. Start a terminal session and start a Truffle Ganache node running `truffle develop`. This will start a Ganache node and create a Truffle console session.
1. In the truffle console, run all migrations from scratch with `migrate --reset`. Keep this terminal session open to maintain your Ganache node running.

#### Snapshots ####
Snapshots are compared as part of the test suite, to check any changes made to the Dapp are sensible. If you change the Dapp, you also need to update the snapshots.
1. `yarn jest -u`
1. or if using npm: `npm run test:app -- -u`

#### Build the permissioning Dapp for deployment ####

1. [Compile and migrate the contracts](#compile-and-migrate-the-contracts)
1. Run `yarn run build` will assemble index.html and all other files in `build/`
1. You can use your preferred web server technology to serve the contents of `build/` as static files.
1. You will need to set up MetaMask as for [the development server](#start-the-development-server)

## Deployment

### Deploying the contracts
1. The following additional environment variables are optional and can be used to prevent redeployment of rules contracts. If set to true, that contract will not be redeployed and current list data will be preserved. If absent or not set to `true`, the specified contract will be redeployed. This allows you, for instance, to retain the Admin contract while redeploying NodeRules and AccountRules, or any other combination.
  - `RETAIN_ADMIN_CONTRACT=true`
  - `RETAIN_NODE_RULES_CONTRACT=true`
  - `RETAIN_ACCOUNT_RULES_CONTRACT=true`
1. The following additional environment variables are optional and can be used to permit accounts and nodes during initial contract deployment
  - `INITIAL_ADMIN_ACCOUNTS`: The admin account addresses. Comma-separated multiple addresses can be specified
  - `INITIAL_ALLOWLISTED_ACCOUNTS`: The permitted account addresses. Comma-separated multiple addresses can be specified
  - `INITIAL_ALLOWLISTED_NODES`: The enode URLs of permitted nodes. Comma-separated multiple nodes can be specified
1. With these environment variables provided, run `truffle migrate --reset` to deploy the contracts. The console will display the addresses of the contracts deployed. 

### Deploying the Dapp
1. Fork or download this repo
1. Run yarn install and them yarn build
1. Copy the .env.sample to .env and edt the file with the proper data:

_Note: The `NODE_INGRESS` and `ACCOUNT_INGRESS` contract addresses MUST match the genesis.json file._
```
NODE_INGRESS_CONTRACT_ADDRESS=0x0000000000000000000000000000000000009999
ACCOUNT_INGRESS_CONTRACT_ADDRESS=0x0000000000000000000000000000000000008888
BESU_NODE_PERM_ACCOUNT=<put admin account addresses here>
BESU_NODE_PERM_KEY=<put private key here>
BESU_NODE_PERM_ENDPOINT=<put RPC URL here> 
CHAIN_ID=<put chainID here>
INITIAL_ALLOWLISTED_NODES=<put enode list here> 
```
1. Use a webserver of your choice to host the contents of the folder as static files directing root requests to `index.html`
