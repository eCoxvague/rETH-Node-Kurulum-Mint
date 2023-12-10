# rETH-Node-Kurulum-Mint

rETH üstünde node kurarak mint yapmayı göstereceğim.

Kodlar 10 puanlık zorluk derecesine göre düzenlenmiştir.



- Orjinal rehbere burdan gidebilirsiniz. 

 - https://reth.gitbook.io/rerc20/what-is-rare-eth-reth/how-to-work

1. Adım

   
Bilgisayarınızda aşağıdaki araçların yüklü olduğundan emin olun

 - Node.js
 - https://go.dev/doc/install


2. Adım 

 - Masaüstüne `mine_rETH` adında klasör oluşturun.

3. Adım

 - Visual Studio açın ve open folder diyerek oluşturduğumuz klasörü açın.
 - Sol tarafda boşluk alanda sağ tıklayarak `yeni dosya` oluştura basın ve adını `package.json` koyun. İçerisine aşağıda kodu ekleyin.

```
 {
 "dependencies": {
 "ethers": "^5.0.0"
 }
}
```

 - `npm install` komutunu terminal üstünde çalıştırın.

4. Adım

 - Geth kurulumu yapacağız. Kurulumlar macOS desteklemektedir. Windows için kurulum farklıdır ve yüklenen videoya bakmanız gereklidir.
 - Aşağıdaki kodları sırayla terminal üstünde yazın.

```
git clone https://github.com/ethereum/go-ethereum.git
```
```
cd go-ethereum
```
```
make geth
```
 - Eğer make geth komutunda hata alıyorsanız bu kodu çalıştırın `xcode-select --install`

5. Adım

 - Web 3 ETH kurulumu yapacağız. Aşağıdaki kodları sırayla terminal üstünde yazın.
```
npm install web3-eth
```
```
yarn add web3-eth
```
 - Eğer yarn kodunda hata alırsanız bu kodu çalıştırın `npm install --global yarn`

6. Adım

 - Sol tarafta boşluk bir alanda sağ tıklayın ve yeni dosya oluştura basın ve adını `mine_rETH.js` koyun. Aşağı kodları içine yapıştırın.

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
 - Burda yapıştırdığınız kod içinde 3. ve 4. satırdaki yerleri kendinize göre değiştirin.
 - 42'nci satırdaki gasLimit: 12000 kısmı yaklaşık 20 25 gwei civarına denk gelmektedir ve 1.5 1.7 dolar civarı fee ücreti keser. Burayı kendinize göre değiştirin gwei yüksekse
   eğer işlem başarısız olur. Merak etmeyin fee ücreti kesmez. İşleminiz geçerli olmaz.

   - Dip Not : Gwei 60 ise ortalama 65000 yaparsanız işleminiz geçerli olur ve 4 5 dolar civarında fee ödersiniz.
  

 - Bütün işlemleri doğru bir şekilde yaptıktan sonra ctrl s yada file save yaparak kaydetmeyi unutmayın.


7. Adım

 - Aşağıdaki kodu çalıştırarak basım işlemini başlatınız.
```
node mine_rETH.js
```

 - Almanız gereken görüntü bu şekildedir.


<img width="963" alt="Ekran Resmi 2023-11-29 01 31 37" src="https://github.com/eCoxvague/rETH-Node-Kurulum-Mint/assets/100167495/8b75392b-4f80-4817-a046-e85dc85d99a1">



 - Bastıklarınızı kontrol etmek isterseniz aşağıki dune sitelerine giderek bakabilirsiniz. Doğru bir basımdan sonra yaklaşık 3 5 dakika sonra işleminiz dune tarafında görünür merak etmeyin.


  - https://dune.com/qihai613/reth
  - https://dune.com/sslisen/reth-mining-indexing

















   
