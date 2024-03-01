### Understanding Ethereum POS 

## How do transactions start and how are they processed in Ethereum?

Transaction Creation

The wallet software takes the user's input and constructs a transaction object.
A user creates and signs a transaction with their private key. 
This is usually handled by a wallet or a library such as ether.js, web3js, web3py etc but under the hood the user is making a request to a node 
using the Ethereum JSON-RPC API. 

This object contains all the necessary fields:
nonce: To prevent replay attacks
gasPrice
gasLimit
to: Recipient address
value: Amount of ETH transferred
data: (Optional) 


## Wallet Software (Hypothetical):

// User provides transaction details (to, value, gasPrice, gasLimit,data)
transaction = NewTransaction()

// Set transaction properties based on user input
transaction.SetTo(toAddress)
transaction.SetValue(value)
transaction.SetGasPrice(gasPrice)
transaction.SetGasLimit(gasLimit)
transaction.SetData(data)

// Get the next valid nonce for the user's account
nonce = GetNextNonce(userAddress)
transaction.SetNonce(nonce)

// Sign the transaction using the user's private key
signature = SignTransaction(transaction, privateKey)
transaction.SetSignature(signature)

// Broadcast the signed transaction to a connected Ethereum node
BroadcastTransaction(transaction)


## Geth (Hypothetical):

The core/types/transaction_signing.go file within the Geth repository is a crucial component in understanding 
how Ethereum transactions are handled,particularly regarding the signing process. 

Let's break down why it's important:

Key Functions in transaction_signing.go:

Sender(tx *Transaction) (common.Address, error): This function is responsible for deriving the sender's Ethereum address from the transaction's signature. 
This is essential for verifying that the person sending the transaction actually owns the account they're sending from.

SignatureValues(tx *Transaction, sig []byte) (R, S, V *big.Int, err error): This function extracts the raw R, S, and V values from an ECDSA signature. 
These values are components of the cryptographic signature.

ChainID() *big.Int: Retrieves the chain ID, which protects against replay attacks across different Ethereum-based chains.

Hash(tx *Transaction) common.Hash: Calculates the transaction hash. 
This hash is the unique identifier of the transaction and is what gets signed with the user's private key.
