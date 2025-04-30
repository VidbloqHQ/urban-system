### WalletAdapterBridge

Bridges the Solana wallet adapter with the SDK's internal wallet context.

```jsx
import { WalletAdapterBridge } from '@vidbloq/react';
import { WalletProvider } from '@solana/wallet-adapter-react';

function App() {
  return (
    <WalletProvider wallets={[/* your wallet adapters */]}>
      <VidbloqProvider apiKey="YOUR_API_KEY" apiSecret="YOUR_API_SECRET">
        <WalletAdapterBridge />
        {/* Your application components */}
      </VidbloqProvider>
    </WalletProvider>
  );
}
```

#### Description

The `WalletAdapterBridge` component:

- Connects the Solana wallet adapter system to the Vidbloq wallet context
- Creates a consistent wallet interface for the SDK to use
- Registers the wallet adapter as a signer for transactions
- Automatically synchronizes connection state

This component should be included once in your application, after both the `WalletProvider` from Solana and the `VidbloqProvider`.