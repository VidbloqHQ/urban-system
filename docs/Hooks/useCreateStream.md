### useCreateStream

Creates a new stream with specified parameters.

```jsx
import { useCreateStream } from '@vidbloq/react';

function CreateStreamComponent() {
  const { createStream, isLoading, error, stream } = useCreateStream();
  
  const handleCreateStream = async () => {
    const newStream = await createStream({
      wallet: 'your-wallet-address',
      callType: 'video',
      title: 'My New Stream',
      streamSessionType: 'livestream',
      isPublic: true
    });
    
    if (newStream) {
      console.log('Stream created:', newStream);
    }
  };
  
  return (
    <div>
      <button onClick={handleCreateStream} disabled={isLoading}>
        {isLoading ? 'Creating...' : 'Create Stream'}
      </button>
      
      {error && <p>Error: {error.message}</p>}
      {stream && <p>Stream created with ID: {stream.id}</p>}
    </div>
  );
}
```

#### Return Value

| Property | Type | Description |
|----------|------|-------------|
| createStream | (data: CreateStreamRequest) => PromiseCreateStreamResponse \| null | Function to create a stream |
| isLoading | boolean | Whether create request is in progress |
| error | Error \| null | Any error that occurred |
| stream | CreateStreamResponse \| null | Created stream data |

#### CreateStreamRequest

| Property | Type | Required | Default | Description |
|----------|------|----------|---------|-------------|
| wallet | string | Yes | - | Creator's wallet address |
| callType | "video" \| "audio" | No | undefined | Type of call |
| scheduledFor | string \| Date | No | undefined | Future scheduled time |
| title | string | No | undefined | Stream title |
| streamSessionType | "livestream" \| "meeting" | No | undefined | Type of stream session |
| fundingType | "free" \| "paid" \| "subscription" | No | undefined | Funding type |
| isPublic | boolean | No | undefined | Whether stream is public |

#### Description

The `useCreateStream` hook provides functionality to create a new stream. It handles:

- API request to create the stream
- Loading and error states
- Stream data storage

This hook is typically used in stream creation forms or wizards.