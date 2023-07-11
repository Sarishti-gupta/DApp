#Dapp
This Solidity program is a simple app that demonstrates the basic syntax and functionality of the Solidity programming language. The purpose of this program is to serve as a starting point for those who are new to Solidity and want to get a feel for how it works.

Description
This program is a simple contract written in Solidity, a programming language used for developing smart contracts on the Ethereum blockchain. The contract has a single function that returns the string "Hello World!". This program serves as a simple and straightforward introduction to Solidity programming, and can be used as a stepping stone for more complex projects in the future.

Getting Started
Executing program
To run this program, you can use Remix, an online Solidity IDE. To get started, go to the Remix website at https://remix.ethereum.org/.

Once you are on the Remix website, create a new file by clicking on the "+" icon in the left-hand sidebar. Save the file with a .sol extension (e.g., HelloWorld.sol). Copy and paste the following code into the file:

async function initWallet(){
    try {
      // check if any wallet provider is installed. i.e metamask xdcpay etc
      setWalletConnectLoader(true)
      if (typeof window.ethereum === 'undefined') {
        console.log("Please install wallet.")
        alert("Please install wallet.")
        setWalletConnectLoader(false)
        return
      }
      else{
          // raise a request for the provider to connect the account to our website
          const web3ModalVar = new Web3Modal({
            cacheProvider: true,
            providerOptions: {
            walletconnect: {
              package: WalletConnectProvider,
            },
          },
        });
        
        const instanceVar = await web3ModalVar.connect();
        const providerVar = new ethers.providers.Web3Provider(instanceVar);
        setProvider(providerVar)
        const signer = providerVar.getSigner();
        const signerAddress = await signer.getAddress()
        console.log(signerAddress)
        setConnectedWalletAddress(signerAddress)
        // readNumber(providerVar)
        setWalletConnected(true)
        setWalletConnectLoader(false)
        return
      }

    } catch (error) {
      console.log(error)
      setWalletConnectLoader(false)
      return
    }
  }
  async function readNumber(provider){
    try {
      setRetrieveLoader(true)
      const signer = provider.getSigner();
  
      // initialize smartcontract with the essentials details.
      const smartContract = new ethers.Contract(contractAddress, abi, provider);
      const contractWithSigner = smartContract.connect(signer);
  
      // interact with the methods in smart contract
      const response = await contractWithSigner.readNum();
  
      console.log(parseInt(response))
      setStoredNumber(parseInt(response))
      setRetrieveLoader(false)
      return
    } catch (error) {
      alert(error)
      setRetrieveLoader(false)
      return
    }
  }
  async function writeNumber(){
    try {
      setStoreLoader(true)
      const signer = provider.getSigner();
      const smartContract = new ethers.Contract(contractAddress, abi, provider);
      const contractWithSigner = smartContract.connect(signer);

      // interact with the methods in smart contract as it's a write operation, we need to invoke the transaction using .wait()
      const writeNumTX = await contractWithSigner.writeNum(enteredNumber);
      const response = await writeNumTX.wait()
      console.log(await response)
      setStoreLoader(false)

      alert(`Number stored successfully ${enteredNumber}`)   
      return

    } catch (error) {
      alert(error)
      setStoreLoader(false)
      return
    }
  }
