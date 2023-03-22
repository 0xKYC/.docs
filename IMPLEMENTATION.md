0xKYC Implementation Documentation
========

To implement 0xKYC all you need is a simple "token gating" system added to your app and you can ensure your users are not OFAC sanctioned and are unique! This is signalled with a 0xKYC soulbound token (SBT). Below is the code needed to get started and implement your new feature! (we will demo this in JS + web3.js)

Goerli testnet soulbound contract address:

    0x275D4440342272dB27be480B127410C8bbf78e14
    
Goerli testnet verifier contract address:

    0x245C4D4b735d034121d096CC5b8d10a80Fdf44c4
    
Mumbai testnet soulbound contract address:

    0x0FbA0f5bBEc4e5De96d0a7765D882279519ACE92
    
Mumbai testnet verifier contract address:

    0x231d35700C10FB2D2F0Ec4f1bc9EeDE024A005fc
    
Scroll Alpha testnet soulbound contract address:

    0x0FbA0f5bBEc4e5De96d0a7765D882279519ACE92
    
Scroll Alpha testnet verifier contract address:

    0x231d35700C10FB2D2F0Ec4f1bc9EeDE024A005fc
  
Wallet address that has 0xKYC soulbound on all above networks:

    0x1dd473E7bC928dC5b9d72d05342117288CB5b511
    
    


3 step integration (Frontend Token-gate):
--------
*NOTE: this implementation will not token gate the smart contract side, please contact to discuss architecture and deeper token gates!* 

 Firstly, define the <code>ABI</code> varable for our contract: 
  <!-- have to be followed by an empty line! -->

 ```javascript
const abi = [ { "inputs": [ { "internaltype": "string", "name": "name_", "type": "string" }, { "internaltype": "string", "name": "symbol_", "type": "string" } ], "statemutability": "nonpayable", "type": "constructor" }, { "anonymous": false, "inputs": [ { "indexed": false, "internaltype": "address", "name": "_soul", "type": "address" } ], "name": "burn", "type": "event" }, { "anonymous": false, "inputs": [ { "indexed": false, "internaltype": "address", "name": "_soul", "type": "address" } ], "name": "mint", "type": "event" }, { "anonymous": false, "inputs": [ { "indexed": true, "internaltype": "address", "name": "previousowner", "type": "address" }, { "indexed": true, "internaltype": "address", "name": "newowner", "type": "address" } ], "name": "ownershiptransferred", "type": "event" }, { "anonymous": false, "inputs": [ { "indexed": false, "internaltype": "address", "name": "_soul", "type": "address" } ], "name": "update", "type": "event" }, { "inputs": [], "name": "_name", "outputs": [ { "internaltype": "string", "name": "", "type": "string" } ], "statemutability": "view", "type": "function" }, { "inputs": [], "name": "_symbol", "outputs": [ { "internaltype": "string", "name": "", "type": "string" } ], "statemutability": "view", "type": "function" }, { "inputs": [], "name": "_totalsbt", "outputs": [ { "internaltype": "uint256", "name": "", "type": "uint256" } ], "statemutability": "view", "type": "function" }, { "inputs": [ { "internaltype": "address", "name": "_soul", "type": "address" } ], "name": "burn", "outputs": [], "statemutability": "nonpayable", "type": "function" }, { "inputs": [ { "internaltype": "address", "name": "_soul", "type": "address" } ], "name": "getsbtdata", "outputs": [ { "internaltype": "uint256[2]", "name": "", "type": "uint256[2]" }, { "internaltype": "uint256[2][2]", "name": "", "type": "uint256[2][2]" }, { "internaltype": "uint256[2]", "name": "", "type": "uint256[2]" }, { "internaltype": "uint256[3]", "name": "", "type": "uint256[3]" } ], "statemutability": "view", "type": "function" }, { "inputs": [ { "internaltype": "address", "name": "_soul", "type": "address" } ], "name": "hassoul", "outputs": [ { "internaltype": "bool", "name": "", "type": "bool" } ], "statemutability": "view", "type": "function" }, { "inputs": [ { "internaltype": "uint256[2]", "name": "a", "type": "uint256[2]" }, { "internaltype": "uint256[2][2]", "name": "b", "type": "uint256[2][2]" }, { "internaltype": "uint256[2]", "name": "c", "type": "uint256[2]" }, { "internaltype": "uint256[3]", "name": "input", "type": "uint256[3]" }, { "internaltype": "address", "name": "to", "type": "address" } ], "name": "mint", "outputs": [], "statemutability": "nonpayable", "type": "function" }, { "inputs": [], "name": "name", "outputs": [ { "internaltype": "string", "name": "", "type": "string" } ], "statemutability": "view", "type": "function" }, { "inputs": [], "name": "owner", "outputs": [ { "internaltype": "address", "name": "", "type": "address" } ], "statemutability": "view", "type": "function" }, { "inputs": [], "name": "renounceownership", "outputs": [], "statemutability": "nonpayable", "type": "function" }, { "inputs": [], "name": "symbol", "outputs": [ { "internaltype": "string", "name": "", "type": "string" } ], "statemutability": "view", "type": "function" }, { "inputs": [], "name": "totalsbt", "outputs": [ { "internaltype": "uint256", "name": "", "type": "uint256" } ], "statemutability": "view", "type": "function" }, { "inputs": [ { "internaltype": "address", "name": "newowner", "type": "address" } ], "name": "transferownership", "outputs": [], "statemutability": "nonpayable", "type": "function" }, { "inputs": [ { "internaltype": "address", "name": "_soul", "type": "address" }, { "components": [ { "internaltype": "uint256[2]", "name": "a", "type": "uint256[2]" }, { "internaltype": "uint256[2][2]", "name": "b", "type": "uint256[2][2]" }, { "internaltype": "uint256[2]", "name": "c", "type": "uint256[2]" }, { "internaltype": "uint256[3]", "name": "input", "type": "uint256[3]" } ], "internaltype": "struct zksbt. Proof", "name": "_souldata", "type": "tuple" } ], "name": "updatesbt", "outputs": [ { "internaltype": "bool", "name": "", "type": "bool" } ], "statemutability": "nonpayable", "type": "function" }, { "inputs": [ { "internaltype": "address", "name": "_soul", "type": "address" }, { "internaltype": "address", "name": "verifieraddress", "type": "address" } ], "name": "validateattribute", "outputs": [ { "internaltype": "bool", "name": "", "type": "bool" } ], "statemutability": "view", "type": "function" } 
```



Secondly, call the method  <code>.hasSoul()</code> to check a wallet for soulbound:

```javascript
const soulbound = new web3.eth.Contract(abi, soulboundContractAddress);
const hasSoul = await soulbound.methods.hasSoul(walletAddress).call();
   ```

Lastly, token gate the key functionality of your protocol with a check for our SBT:

```javascript
    const checkSBT = async () => {
      try {
        if (address) {
          const isVerified = await checkForSBT(address);

          if (isVerified) {
            setIsAuth(true);
          } else {
            setIsAuth(false);
          }
        }
      } catch (err) {
        // handle error
      }
    };
   ```



Smart Contract Implementation (Solidity Modifier):
--------

Here is an example of a solidity modifier that gates a core function, msg.sender must have a 0xKYC soulbound 

```solidity

interface 0xKYC {
    function hasSoul(address _soul) external view returns (bool);
}

contract YourContractUsing0xKYC {
    0xKYC public my0xKYC;

    constructor(address 0xKYCAddress) {
        my0xKYC = 0xKYC(0xKYCAddress);
    }

    modifier hasSoul {
        require(my0xKYC.hasSoul(msg.sender), "Soul does not exist");
        _;
    }

    function coreFunction() public hasSoul {
        // do something
    }
   ```



On chain verification
--------
<h3> With Code (JS)</h3>


 Define ABI for both verifier and 0xKYC soulbound contracts (<i>click to expand</i>)
        
 ```javascript
        
 const verifierAbi = [{"inputs":[{"components":[{"components":[{"internalType":"uint256","name":"X","type":"uint256"},{"internalType":"uint256","name":"Y","type":"uint256"}],"internalType":"struct Pairing.G1Point","name":"a","type":"tuple"},{"components":[{"internalType":"uint256[2]","name":"X","type":"uint256[2]"},{"internalType":"uint256[2]","name":"Y","type":"uint256[2]"}],"internalType":"struct Pairing.G2Point","name":"b","type":"tuple"},{"components":[{"internalType":"uint256","name":"X","type":"uint256"},{"internalType":"uint256","name":"Y","type":"uint256"}],"internalType":"struct Pairing.G1Point","name":"c","type":"tuple"}],"internalType":"struct ZkVerifier.Proof","name":"proof","type":"tuple"},{"internalType":"uint256[3]","name":"input","type":"uint256[3]"}],"name":"verifyTx","outputs":[{"internalType":"bool","name":"r","type":"bool"}],"stateMutability":"view","type":"function"}]
        
        
const soulboundAbi = [ { "inputs": [ { "internaltype": "string", "name": "name_", "type": "string" }, { "internaltype": "string", "name": "symbol_", "type": "string" } ], "statemutability": "nonpayable", "type": "constructor" }, { "anonymous": false, "inputs": [ { "indexed": false, "internaltype": "address", "name": "_soul", "type": "address" } ], "name": "burn", "type": "event" }, { "anonymous": false, "inputs": [ { "indexed": false, "internaltype": "address", "name": "_soul", "type": "address" } ], "name": "mint", "type": "event" }, { "anonymous": false, "inputs": [ { "indexed": true, "internaltype": "address", "name": "previousowner", "type": "address" }, { "indexed": true, "internaltype": "address", "name": "newowner", "type": "address" } ], "name": "ownershiptransferred", "type": "event" }, { "anonymous": false, "inputs": [ { "indexed": false, "internaltype": "address", "name": "_soul", "type": "address" } ], "name": "update", "type": "event" }, { "inputs": [], "name": "_name", "outputs": [ { "internaltype": "string", "name": "", "type": "string" } ], "statemutability": "view", "type": "function" }, { "inputs": [], "name": "_symbol", "outputs": [ { "internaltype": "string", "name": "", "type": "string" } ], "statemutability": "view", "type": "function" }, { "inputs": [], "name": "_totalsbt", "outputs": [ { "internaltype": "uint256", "name": "", "type": "uint256" } ], "statemutability": "view", "type": "function" }, { "inputs": [ { "internaltype": "address", "name": "_soul", "type": "address" } ], "name": "burn", "outputs": [], "statemutability": "nonpayable", "type": "function" }, { "inputs": [ { "internaltype": "address", "name": "_soul", "type": "address" } ], "name": "getsbtdata", "outputs": [ { "internaltype": "uint256[2]", "name": "", "type": "uint256[2]" }, { "internaltype": "uint256[2][2]", "name": "", "type": "uint256[2][2]" }, { "internaltype": "uint256[2]", "name": "", "type": "uint256[2]" }, { "internaltype": "uint256[3]", "name": "", "type": "uint256[3]" } ], "statemutability": "view", "type": "function" }, { "inputs": [ { "internaltype": "address", "name": "_soul", "type": "address" } ], "name": "hassoul", "outputs": [ { "internaltype": "bool", "name": "", "type": "bool" } ], "statemutability": "view", "type": "function" }, { "inputs": [ { "internaltype": "uint256[2]", "name": "a", "type": "uint256[2]" }, { "internaltype": "uint256[2][2]", "name": "b", "type": "uint256[2][2]" }, { "internaltype": "uint256[2]", "name": "c", "type": "uint256[2]" }, { "internaltype": "uint256[3]", "name": "input", "type": "uint256[3]" }, { "internaltype": "address", "name": "to", "type": "address" } ], "name": "mint", "outputs": [], "statemutability": "nonpayable", "type": "function" }, { "inputs": [], "name": "name", "outputs": [ { "internaltype": "string", "name": "", "type": "string" } ], "statemutability": "view", "type": "function" }, { "inputs": [], "name": "owner", "outputs": [ { "internaltype": "address", "name": "", "type": "address" } ], "statemutability": "view", "type": "function" }, { "inputs": [], "name": "renounceownership", "outputs": [], "statemutability": "nonpayable", "type": "function" }, { "inputs": [], "name": "symbol", "outputs": [ { "internaltype": "string", "name": "", "type": "string" } ], "statemutability": "view", "type": "function" }, { "inputs": [], "name": "totalsbt", "outputs": [ { "internaltype": "uint256", "name": "", "type": "uint256" } ], "statemutability": "view", "type": "function" }, { "inputs": [ { "internaltype": "address", "name": "newowner", "type": "address" } ], "name": "transferownership", "outputs": [], "statemutability": "nonpayable", "type": "function" }, { "inputs": [ { "internaltype": "address", "name": "_soul", "type": "address" }, { "components": [ { "internaltype": "uint256[2]", "name": "a", "type": "uint256[2]" }, { "internaltype": "uint256[2][2]", "name": "b", "type": "uint256[2][2]" }, { "internaltype": "uint256[2]", "name": "c", "type": "uint256[2]" }, { "internaltype": "uint256[3]", "name": "input", "type": "uint256[3]" } ], "internaltype": "struct zksbt. Proof", "name": "_souldata", "type": "tuple" } ], "name": "updatesbt", "outputs": [ { "internaltype": "bool", "name": "", "type": "bool" } ], "statemutability": "nonpayable", "type": "function" }, { "inputs": [ { "internaltype": "address", "name": "_soul", "type": "address" }, { "internaltype": "address", "name": "verifieraddress", "type": "address" } ], "name": "validateattribute", "outputs": [ { "internaltype": "bool", "name": "", "type": "bool" } ], "statemutability": "view", "type": "function" } ]
    
```  

Next, get proof from the wallet address you want to verifiy by calling the method <code>.getSBTData</code> inside the soulbound contract and then call <code>.verifyTx</code> from the verifier contract with this proof data (<i>click to expand</i>)

```javascript
    const soulbound = new web3.eth.Contract(soulboundAbi, soulboundContractAddress);
    const verifier = new web3.eth.Contract(verifierAbi, soulboundContractAddress);
    const sbtData = await soulbound.methods.getSBTData(walletAddress).call();
    const proof = [sbtData[0], sbtData[1], sbtData[2]];
    const inputs = sbtData[3];
    const isVerified = await verifier.methods.verifyTx(proof, inputs).call();
```


<h3> Without Code (etherscan) </h3>

- Find to the soulbound contract on etherscan and go to read contract, paste an address you know has a soulbound into <code> .getSBTData </code> to get their zk proof, copy the response.

<img src="https://image-hosting-0xkyc.s3.amazonaws.com/Screenshot+2023-01-23+at+12.30.51.png" >

- Then find the verifier contract on etherscan and paste this proof to check if users proof is valid!

<img src="https://image-hosting-0xkyc.s3.amazonaws.com/Screenshot+2023-01-23+at+12.38.42.png">


Support
-------

If you are having issues, please let us know.
We have a mailing list located at: support@0xkyc.id

<!-- License
-------

The project is licensed under the __ license. -->
