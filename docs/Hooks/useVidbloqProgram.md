### useVidbloqProgram

Provides access to Vidbloq's on-chain program for blockchain interactions.

```jsx
import { useVidbloqProgram } from '@vidbloq/react';

function ProgramInteractionComponent() {
  const { 
    program, 
    isInitialized, 
    createStream, 
    updateStream, 
    deleteStream, 
    error 
  } = useVidbloqProgram();
  
  // Use program functions for on-chain operations
  const handleCreateOnChainStream = async () => {
    if (!isInitialized) return;
    
    try {
      const streamId = await createStream({
        name: 'My On-Chain Stream',
        description: 'This stream is recorded on the blockchain',
        price: 0.1,
        isPublic: true
      });
      
      console.log('Stream created with ID:', streamId);
    } catch (err) {
      console.error('Failed to create on-chain stream:', err);
    }
  };
  
  return (
    <div>
      <button 
        onClick={handleCreateOnChainStream}
        disabled={!isInitialized}
      >
        Create On-Chain Stream
      </button>
      
      {error && <p>Error: {error.message}</p>}
    </div>
  );
}
```

#### Return Value

| Property | Type | Description |
|----------|------|-------------|
| program | Program \| null | The Vidbloq program instance |
| isInitialized | boolean | Whether the program is initialized |
| createStream | (data: CreateStreamData) => Promise string | Function to create on-chain stream |
| updateStream | (id: string, data: UpdateStreamData) => Promise boolean | Function to update on-chain stream |
| deleteStream | (id: string) => Promise boolean | Function to delete on-chain stream |
| error | Error \| null | Any error that occurred during initialization |

#### Description

The `useVidbloqProgram` hook provides access to Vidbloq's blockchain program for on-chain operations. It:

- Initializes the program connection
- Provides methods for common on-chain operations
- Manages program state and errors

This hook is used for applications that need to interact with the blockchain for stream creation, payments, or other on-chain features.