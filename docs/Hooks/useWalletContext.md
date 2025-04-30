### useWalletContext

Provides access to the connected wallet and signing functionality.

```jsx
import { useWalletContext } from '@vidbloq/react';

function MyWalletComponent() {
  const { 
    publicKey, 
    connectWallet, 
    clearWallet, 
    signTransaction, 
    connected 
  } = useWalletContext();
  
  return (
    <div>
      {publicKey ? 
        <p>Connected: {publicKey.toString()}</p> : 
        <button onClick={() => /* connect wallet */}>Connect Wallet</button>
      }
    </div>
  );
}
```

#### Return Value

| Property | Type | Description |
|----------|------|-------------|
| publicKey | PublicKey \| null | Public key of the connected wallet |
| connectWallet | (key: PublicKey, signer?: WalletSigner) => void | Function to connect a wallet |
| clearWallet | () => void | Function to disconnect wallet |
| signTransaction | ((transaction: Transaction) => Promise Transaction) \| undefined | Function to sign transactions |
| connected | boolean | Whether wallet is fully connected with signing capability |

#### Description

The `useWalletContext` hook provides access to the wallet context created by the `WalletProvider` component. It offers a unified interface for wallet interactions regardless of whether using the SDK's built-in wallet system or an external adapter like Solana wallet adapter.