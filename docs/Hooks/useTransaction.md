### useTransaction

Facilitates cryptocurrency transactions using the connected wallet.

```jsx
import { useTransaction, useWalletContext } from '@vidbloq/react';
import { PublicKey } from '@solana/web3.js';

function TransactionComponent() {
  const { publicKey } = useWalletContext();
  
  const recipients = [
    {
      publicKey: new PublicKey('recipient-address'),
      amount: 0.1 // SOL amount
    }
  ];
  
  const { 
    fetchTransaction, 
    signAndSubmitTransaction, 
    transactionBase64, 
    transactionSignature, 
    error, 
    loading 
  } = useTransaction({
    recipients,
    tokenName: 'sol'
  });
  
  const handleSendTransaction = async () => {
    try {
      await fetchTransaction();
      await signAndSubmitTransaction();
      console.log('Transaction successful:', transactionSignature);
    } catch (err) {
      console.error('Transaction failed:', err);
    }
  };
  
  return (
    <div>
      <button onClick={handleSendTransaction} disabled={loading || !publicKey}>
        {loading ? 'Processing...' : 'Send SOL'}
      </button>
      
      {error && <p>Error: {error}</p>}
      {transactionSignature && <p>Transaction complete!</p>}
    </div>
  );
}
```

#### Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| props | UseTransactionProps | Yes | - | Transaction configuration |

#### UseTransactionProps

| Property | Type | Required | Default | Description |
|----------|------|----------|---------|-------------|
| recipients | Recipient[] | Yes | - | List of transaction recipients |
| tokenName | string | No | 'sol' | Token to send (default: SOL) |

#### Recipient

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| publicKey | PublicKey | Yes | Recipient's public key |
| amount | number | Yes | Amount to send |

#### Return Value

| Property | Type | Description |
|----------|------|-------------|
| fetchTransaction | () => Promise void | Function to fetch transaction data |
| signAndSubmitTransaction | () => Promise void | Function to sign and submit transaction |
| transactionBase64 | string \| null | Base64-encoded transaction data |
| transactionSignature | string \| null | Transaction signature after submission |
| error | string \| null | Any error that occurred |
| loading | boolean | Whether transaction is in progress |

#### Description

The `useTransaction` hook facilitates cryptocurrency transactions. It separates the transaction process into two steps:

1. `fetchTransaction`: Retrieves transaction data from the server
2. `signAndSubmitTransaction`: Signs the transaction with the connected wallet and submits it

This hook is used for tipping, payments, and other financial interactions within streams.