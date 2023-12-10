rETH-Node-Installation-Mint

The codes were organised according to 10-point difficulty.


I will show you how to make mint by installing node on rETH.


- You can go to the original guide here.

- https://reth.gitbook.io/rerc20/what-is-rare-eth-reth/how-to-work


Step 1

- Make sure you have the following tools installed on your computer


  - Node.js
  - https://go.dev/doc/install

Step 2

- Create a folder named `mine_rETH` on the desktop.


Step 3 

- Open Visual Studio and open the folder we created by saying open folder.On the left side, right click on the empty space and press `create new` file and name it `package.json`. Add the following code inside.
```
{
 "dependencies": {
 "ethers": "^5.0.0"
 }
}
```

- Run the `npm install` command on the terminal.


Step 4 


- We will install Geth. Installations support macOS. For Windows the installation is different and you need to look at the uploaded video.
- Type the following codes sequentially on the terminal.

```
git clone https://github.com/ethereum/go-ethereum.git
```
```
cd go-ethereum
```
```
make geth
```

- If you get an error with the make geth command, run this code `xcode-select --install`


Step 5 

- We will install Web 3 ETH. Type the following codes on the terminal in order.
```
npm install web3-eth
```
```
yarn add web3-eth
```

- If you get an error with yarn, run this code `npm install --global yarn`

  
Step 6 

- Right click on a blank area on the left side and press create new file and name it `mine_rETH.js`. Paste the following codes into it.
```
const { ethers } = require('ethers');
const provider = new ethers.providers.JsonRpcProvider('https://ethereum.publicnode.com');
const account = 'Your Ethereum Address';
const privateKey = 'Your Private Key';
const wallet = new ethers.Wallet(privateKey, provider);
const currentChallenge = ethers.utils.formatBytes32String('rETH'); //0x7245544800000000000000000000000000000000000000000000000000000000


let solution;


while (1) {
    const random_value = ethers.utils.randomBytes(32);
    const potential_solution = ethers.utils.hexlify(random_value);
    const hashed_solution = ethers.utils.keccak256(ethers.utils.defaultAbiCoder.encode(["bytes32", "bytes32"], [potential_solution, currentChallenge]))
    console.log(hashed_solution)
    if (hashed_solution.startsWith('0x0077777777')) {
        solution = potential_solution;
        break
    }
}


const jsonData = {
    "p": "rerc-20",
    "op": "mint",
    "tick": "rETH",
    "id": solution,
    'amt': "10000"
}


const dataHex = ethers.utils.hexlify(ethers.utils.toUtf8Bytes('data:application/json,' + JSON.stringify(jsonData)));


async function mine_rETH() {
    const nonce = await provider.getTransactionCount(account);
    const gasPrice = await provider.getGasPrice();


    const tx = {
        from: account,
        to: account, // Self-transfer
        nonce: nonce,
        gasPrice: gasPrice,
        data: dataHex,
        chainId: 1,
        gasLimit: 12000,
    }; 




    const signedTx = await wallet.signTransaction(tx);
    const receipt = await provider.sendTransaction(signedTx);
    console.log('Transaction Receipt:', receipt);
}


mine_rETH();
```

- In the code you pasted here, change the places in the 3rd and 4th lines according to yourself.


  - The gasLimit: 12000 part in line 42 corresponds to approximately 20 25 gwei and charges a fee of around 1.5 1.7 dollars. Change here according to yourself If the gwei is high, the transaction will fail. Do not worry, it does not charge a fee. Your transaction will not be valid.


     - Note: If Gwei is 60, if you make an average of 65000, your transaction will be valid and you will pay a fee of around 4 5 dollars.
     - After doing all the operations correctly, do not forget to save by ctrl s or file save.


Step 7

- Start the printing process by running the following code.
```
node mine_rETH.js
```

- This is the image you should get.


<img width="963" alt="Ekran Resmi 2023-11-29 01 31 37" src="https://github.com/eCoxvague/rETH-Node-Kurulum-Mint/assets/100167495/8b75392b-4f80-4817-a046-e85dc85d99a1">


- If you want to check your presses, you can go to the following dune sites. Do not worry, after a correct printing, your process will appear on the dune side after about 3 to 5 minutes.


  - https://dune.com/qihai613/reth
  - https://dune.com/sslisen/reth-mining-indexing
