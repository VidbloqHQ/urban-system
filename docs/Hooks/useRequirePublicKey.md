### useRequirePublicKey

A utility hook that ensures a public key is available, with optional signing capability requirement.

```jsx
import { useRequirePublicKey } from '@vidbloq/react';

function MyComponent() {
  // Will prompt the user to connect their wallet if not connected
  const { publicKey, hasSigningCapability } = useRequirePublicKey(true); // true = require signing capability
  
  // Component logic that requires a wallet
  if (!publicKey) {
    return <div>Connecting wallet...</div>;
  }
  
  return <div>Wallet connected: {publicKey.toString()}</div>;
}
```

#### Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| requireSigning | boolean | No | false | Whether to require signing capability |

#### Return Value

| Property | Type | Description |
|----------|------|-------------|
| publicKey | PublicKey \| null | Public key of the connected wallet |
| hasSigningCapability | boolean | Whether wallet has signing capability |

#### Description

The `useRequirePublicKey` hook is a convenience utility that tries to ensure a wallet is connected. It will:

1. Check if a wallet is already connected
2. If not, try to restore a wallet from local storage
3. Display a notification to the user if wallet connection is needed
4. Optionally verify signing capability

This hook is ideal for components that require wallet authentication.