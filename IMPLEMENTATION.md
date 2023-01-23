0xKYC Implementation Documentation
========

To implement 0xKYC all you need is a simple "token gating" system added to your app and you can ensure your users are not OFAC sanctioned and are unique! This is signalled with a 0xKYC soulbound token (SBT). Below is the code needed to get started and implement your new feature! (we will demo this in JS + web3.js)

Goerli testnet soulbound contract address:

    0x275D4440342272dB27be480B127410C8bbf78e14
    
Goerli testnet verifier contract address:

    0x245C4D4b735d034121d096CC5b8d10a80Fdf44c4


3 step integration:
--------
*NOTE: this implementation will not token gate the smart contract side, please contact to discuss architecture and deeper token gates!* 
<details>
 <summary> <h3> Firstly, define the <code>ABI</code> varable for our contract: </h3> (<i>click to expand</i>)</summary>
  <!-- have to be followed by an empty line! -->

 ```javascript
 const abi = [
        {
          "inputs": [
            {
              "internalType": "string",
              "name": "name_",
              "type": "string"
            },
            {
              "internalType": "string",
              "name": "symbol_",
              "type": "string"
            }
          ],
          "stateMutability": "nonpayable",
          "type": "constructor"
        },
        {
          "anonymous": false,
          "inputs": [
            {
              "indexed": false,
              "internalType": "address",
              "name": "_soul",
              "type": "address"
            }
          ],
          "name": "Burn",
          "type": "event"
        },
        {
          "anonymous": false,
          "inputs": [
            {
              "indexed": false,
              "internalType": "address",
              "name": "_soul",
              "type": "address"
            }
          ],
          "name": "Mint",
          "type": "event"
        },
        {
          "anonymous": false,
          "inputs": [
            {
              "indexed": true,
              "internalType": "address",
              "name": "previousOwner",
              "type": "address"
            },
            {
              "indexed": true,
              "internalType": "address",
              "name": "newOwner",
              "type": "address"
            }
          ],
          "name": "OwnershipTransferred",
          "type": "event"
        },
        {
          "anonymous": false,
          "inputs": [
            {
              "indexed": false,
              "internalType": "address",
              "name": "_soul",
              "type": "address"
            }
          ],
          "name": "Update",
          "type": "event"
        },
        {
          "inputs": [],
          "name": "_name",
          "outputs": [
            {
              "internalType": "string",
              "name": "",
              "type": "string"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [],
          "name": "_symbol",
          "outputs": [
            {
              "internalType": "string",
              "name": "",
              "type": "string"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [],
          "name": "_totalSBT",
          "outputs": [
            {
              "internalType": "uint256",
              "name": "",
              "type": "uint256"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [
            {
              "internalType": "address",
              "name": "_soul",
              "type": "address"
            }
          ],
          "name": "burn",
          "outputs": [],
          "stateMutability": "nonpayable",
          "type": "function"
        },
        {
          "inputs": [
            {
              "internalType": "address",
              "name": "_soul",
              "type": "address"
            }
          ],
          "name": "getSBTData",
          "outputs": [
            {
              "internalType": "uint256[2]",
              "name": "",
              "type": "uint256[2]"
            },
            {
              "internalType": "uint256[2][2]",
              "name": "",
              "type": "uint256[2][2]"
            },
            {
              "internalType": "uint256[2]",
              "name": "",
              "type": "uint256[2]"
            },
            {
              "internalType": "uint256[3]",
              "name": "",
              "type": "uint256[3]"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [
            {
              "internalType": "address",
              "name": "_soul",
              "type": "address"
            }
          ],
          "name": "hasSoul",
          "outputs": [
            {
              "internalType": "bool",
              "name": "",
              "type": "bool"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [
            {
              "internalType": "uint256[2]",
              "name": "a",
              "type": "uint256[2]"
            },
            {
              "internalType": "uint256[2][2]",
              "name": "b",
              "type": "uint256[2][2]"
            },
            {
              "internalType": "uint256[2]",
              "name": "c",
              "type": "uint256[2]"
            },
            {
              "internalType": "uint256[3]",
              "name": "input",
              "type": "uint256[3]"
            },
            {
              "internalType": "address",
              "name": "to",
              "type": "address"
            }
          ],
          "name": "mint",
          "outputs": [],
          "stateMutability": "nonpayable",
          "type": "function"
        },
        {
          "inputs": [],
          "name": "name",
          "outputs": [
            {
              "internalType": "string",
              "name": "",
              "type": "string"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [],
          "name": "owner",
          "outputs": [
            {
              "internalType": "address",
              "name": "",
              "type": "address"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [],
          "name": "renounceOwnership",
          "outputs": [],
          "stateMutability": "nonpayable",
          "type": "function"
        },
        {
          "inputs": [],
          "name": "symbol",
          "outputs": [
            {
              "internalType": "string",
              "name": "",
              "type": "string"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [],
          "name": "totalSBT",
          "outputs": [
            {
              "internalType": "uint256",
              "name": "",
              "type": "uint256"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [
            {
              "internalType": "address",
              "name": "newOwner",
              "type": "address"
            }
          ],
          "name": "transferOwnership",
          "outputs": [],
          "stateMutability": "nonpayable",
          "type": "function"
        },
        {
          "inputs": [
            {
              "internalType": "address",
              "name": "_soul",
              "type": "address"
            },
            {
              "components": [
                {
                  "internalType": "uint256[2]",
                  "name": "a",
                  "type": "uint256[2]"
                },
                {
                  "internalType": "uint256[2][2]",
                  "name": "b",
                  "type": "uint256[2][2]"
                },
                {
                  "internalType": "uint256[2]",
                  "name": "c",
                  "type": "uint256[2]"
                },
                {
                  "internalType": "uint256[3]",
                  "name": "input",
                  "type": "uint256[3]"
                }
              ],
              "internalType": "struct zkSBT.Proof",
              "name": "_soulData",
              "type": "tuple"
            }
          ],
          "name": "updateSBT",
          "outputs": [
            {
              "internalType": "bool",
              "name": "",
              "type": "bool"
            }
          ],
          "stateMutability": "nonpayable",
          "type": "function"
        },
        {
          "inputs": [
            {
              "internalType": "address",
              "name": "_soul",
              "type": "address"
            },
            {
              "internalType": "address",
              "name": "verifierAddress",
              "type": "address"
            }
          ],
          "name": "validateAttribute",
          "outputs": [
            {
              "internalType": "bool",
              "name": "",
              "type": "bool"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        }
      ]
```


</details>

  
    
<details>
 <summary>
<h3> Secondly, call the method  <code>.hasSoul()</code> to check a wallet for soulbound </h3> (<i>click to expand</i>) </summary>

```javascript
const soulbound = new web3.eth.Contract(abi, soulboundContractAddress);
const hasSoul = await soulbound.methods.hasSoul(walletAddress).call();
   ```
</details> 

<details>
 <summary>
<h3> Lastly, token gate the key functionality of your protocol with a check for our SBT </h3> (<i>click to expand</i>) </summary>

```javascript
    const checkSBT = async () => {
      try {
        if (address) {
          setLoading(true);
          const isVerified = await checkForSBT(address);

          if (isVerified) {
            setIsAuth(true);
          } else {
            setIsAuth(false);
          }
          setLoading(false);
        }
      } catch (err) {
        setLoading(false);
        console.error(err);
      }
    };
   ```
</details> 




On chain verification
--------
<h3> With Code (JS)</h3>


 <details> <summary> Define ABI for both verifier and 0xKYC soulbound contracts (<i>click to expand</i>) </summary> 
        
 ```javascript
        
 const verifierAbi = [{"inputs":[{"components":[{"components":[{"internalType":"uint256","name":"X","type":"uint256"},{"internalType":"uint256","name":"Y","type":"uint256"}],"internalType":"struct Pairing.G1Point","name":"a","type":"tuple"},{"components":[{"internalType":"uint256[2]","name":"X","type":"uint256[2]"},{"internalType":"uint256[2]","name":"Y","type":"uint256[2]"}],"internalType":"struct Pairing.G2Point","name":"b","type":"tuple"},{"components":[{"internalType":"uint256","name":"X","type":"uint256"},{"internalType":"uint256","name":"Y","type":"uint256"}],"internalType":"struct Pairing.G1Point","name":"c","type":"tuple"}],"internalType":"struct ZkVerifier.Proof","name":"proof","type":"tuple"},{"internalType":"uint256[3]","name":"input","type":"uint256[3]"}],"name":"verifyTx","outputs":[{"internalType":"bool","name":"r","type":"bool"}],"stateMutability":"view","type":"function"}]
        
        
 const soulboundAbi = [
        {
          "inputs": [
            {
              "internalType": "string",
              "name": "name_",
              "type": "string"
            },
            {
              "internalType": "string",
              "name": "symbol_",
              "type": "string"
            }
          ],
          "stateMutability": "nonpayable",
          "type": "constructor"
        },
        {
          "anonymous": false,
          "inputs": [
            {
              "indexed": false,
              "internalType": "address",
              "name": "_soul",
              "type": "address"
            }
          ],
          "name": "Burn",
          "type": "event"
        },
        {
          "anonymous": false,
          "inputs": [
            {
              "indexed": false,
              "internalType": "address",
              "name": "_soul",
              "type": "address"
            }
          ],
          "name": "Mint",
          "type": "event"
        },
        {
          "anonymous": false,
          "inputs": [
            {
              "indexed": true,
              "internalType": "address",
              "name": "previousOwner",
              "type": "address"
            },
            {
              "indexed": true,
              "internalType": "address",
              "name": "newOwner",
              "type": "address"
            }
          ],
          "name": "OwnershipTransferred",
          "type": "event"
        },
        {
          "anonymous": false,
          "inputs": [
            {
              "indexed": false,
              "internalType": "address",
              "name": "_soul",
              "type": "address"
            }
          ],
          "name": "Update",
          "type": "event"
        },
        {
          "inputs": [],
          "name": "_name",
          "outputs": [
            {
              "internalType": "string",
              "name": "",
              "type": "string"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [],
          "name": "_symbol",
          "outputs": [
            {
              "internalType": "string",
              "name": "",
              "type": "string"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [],
          "name": "_totalSBT",
          "outputs": [
            {
              "internalType": "uint256",
              "name": "",
              "type": "uint256"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [
            {
              "internalType": "address",
              "name": "_soul",
              "type": "address"
            }
          ],
          "name": "burn",
          "outputs": [],
          "stateMutability": "nonpayable",
          "type": "function"
        },
        {
          "inputs": [
            {
              "internalType": "address",
              "name": "_soul",
              "type": "address"
            }
          ],
          "name": "getSBTData",
          "outputs": [
            {
              "internalType": "uint256[2]",
              "name": "",
              "type": "uint256[2]"
            },
            {
              "internalType": "uint256[2][2]",
              "name": "",
              "type": "uint256[2][2]"
            },
            {
              "internalType": "uint256[2]",
              "name": "",
              "type": "uint256[2]"
            },
            {
              "internalType": "uint256[3]",
              "name": "",
              "type": "uint256[3]"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [
            {
              "internalType": "address",
              "name": "_soul",
              "type": "address"
            }
          ],
          "name": "hasSoul",
          "outputs": [
            {
              "internalType": "bool",
              "name": "",
              "type": "bool"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [
            {
              "internalType": "uint256[2]",
              "name": "a",
              "type": "uint256[2]"
            },
            {
              "internalType": "uint256[2][2]",
              "name": "b",
              "type": "uint256[2][2]"
            },
            {
              "internalType": "uint256[2]",
              "name": "c",
              "type": "uint256[2]"
            },
            {
              "internalType": "uint256[3]",
              "name": "input",
              "type": "uint256[3]"
            },
            {
              "internalType": "address",
              "name": "to",
              "type": "address"
            }
          ],
          "name": "mint",
          "outputs": [],
          "stateMutability": "nonpayable",
          "type": "function"
        },
        {
          "inputs": [],
          "name": "name",
          "outputs": [
            {
              "internalType": "string",
              "name": "",
              "type": "string"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [],
          "name": "owner",
          "outputs": [
            {
              "internalType": "address",
              "name": "",
              "type": "address"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [],
          "name": "renounceOwnership",
          "outputs": [],
          "stateMutability": "nonpayable",
          "type": "function"
        },
        {
          "inputs": [],
          "name": "symbol",
          "outputs": [
            {
              "internalType": "string",
              "name": "",
              "type": "string"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [],
          "name": "totalSBT",
          "outputs": [
            {
              "internalType": "uint256",
              "name": "",
              "type": "uint256"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [
            {
              "internalType": "address",
              "name": "newOwner",
              "type": "address"
            }
          ],
          "name": "transferOwnership",
          "outputs": [],
          "stateMutability": "nonpayable",
          "type": "function"
        },
        {
          "inputs": [
            {
              "internalType": "address",
              "name": "_soul",
              "type": "address"
            },
            {
              "components": [
                {
                  "internalType": "uint256[2]",
                  "name": "a",
                  "type": "uint256[2]"
                },
                {
                  "internalType": "uint256[2][2]",
                  "name": "b",
                  "type": "uint256[2][2]"
                },
                {
                  "internalType": "uint256[2]",
                  "name": "c",
                  "type": "uint256[2]"
                },
                {
                  "internalType": "uint256[3]",
                  "name": "input",
                  "type": "uint256[3]"
                }
              ],
              "internalType": "struct zkSBT.Proof",
              "name": "_soulData",
              "type": "tuple"
            }
          ],
          "name": "updateSBT",
          "outputs": [
            {
              "internalType": "bool",
              "name": "",
              "type": "bool"
            }
          ],
          "stateMutability": "nonpayable",
          "type": "function"
        },
        {
          "inputs": [
            {
              "internalType": "address",
              "name": "_soul",
              "type": "address"
            },
            {
              "internalType": "address",
              "name": "verifierAddress",
              "type": "address"
            }
          ],
          "name": "validateAttribute",
          "outputs": [
            {
              "internalType": "bool",
              "name": "",
              "type": "bool"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        }
      ]
    
```  
</details>
<details>
<summary> Next, get proof from the wallet address you want to verifiy by calling the method <code>.getSBTData</code> inside the soulbound contract  and then call <code>.verifyTx</code> from the verifier contract with this proof data (<i>click to expand</i>) </summary>

```javascript
    const soulbound = new web3.eth.Contract(soulboundAbi, soulboundContractAddress);
    const verifier = new web3.eth.Contract(verifierAbi, soulboundContractAddress);
    const sbtData = await soulbound.methods.getSBTData(walletAddress).call();
    const proof = [sbtData[0], sbtData[1], sbtData[2]];
    const inputs = sbtData[3];
    const isVerified = await verifier.methods.verifyTx(proof, inputs).call();
```
</details> 

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
