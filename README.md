# Hyperledger-Fabric-Example-for-a-Simple-Blockchain-
Hyperledger Fabric Example for a Simple Blockchain:
// Hyperledger Fabric SDK installation
const { Gateway, Wallets } = require('fabric-network');
const fs = require('fs');
const path = require('path');

async function main() {
    const gateway = new Gateway();
    const walletPath = path.join(process.cwd(), 'wallet');
    const wallet = await Wallets.newFileSystemWallet(walletPath);

    const ccpPath = path.resolve(__dirname, '..', 'connection.json');
    const ccp = JSON.parse(fs.readFileSync(ccpPath, 'utf8'));

    await gateway.connect(ccp, {
        wallet,
        identity: 'user1',
        discovery: { enabled: false },
    });

    const network = await gateway.getNetwork('mychannel');
    const contract = network.getContract('mycontract');

    // Example: Invoking a smart contract function
    const result = await contract.evaluateTransaction('query', 'arg1');
    console.log(result.toString());
}

main();
