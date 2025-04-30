### useHandleStreamDisconnect

Handles the cleanup process when disconnecting from a stream.

```jsx
import { useHandleStreamDisconnect } from '@vidbloq/react';

function LeaveStreamButton({ walletAddress }) {
  const { leaveStream, isLeaving } = useHandleStreamDisconnect(walletAddress);
  
  return (
    <button 
      onClick={leaveStream}
      disabled={isLeaving}
    >
      {isLeaving ? 'Leaving...' : 'Leave Stream'}
    </button>
  );
}
```

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| walletAddress | string | Yes | User's wallet address |

#### Return Value

| Property | Type | Description |
|----------|------|-------------|
| leaveStream | () => Promise void | Function to leave the stream |
| isLeaving | boolean | Whether leave process is in progress |

#### Description

The `useHandleStreamDisconnect` hook handles the process of properly disconnecting from a stream, including:

- Notifying the server
- Updating participant status
- Cleaning up local resources
- Handling any necessary blockchain operations