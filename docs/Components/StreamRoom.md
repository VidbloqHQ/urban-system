### StreamRoom

Creates a stream room context for hosting or joining a stream.

```jsx
import { StreamRoom, StreamView } from '@vidbloq/react';

function MyStreamRoom() {
  return (
    <StreamRoom roomName="my-awesome-stream">
      <StreamView />
    </StreamRoom>
  );
}
```

#### Props

| Prop | Type | Required | Description |
|------|------|----------|-------------|
| roomName | string | Yes | Unique identifier for the stream room |
| children | React.ReactNode | Yes | Child components |

#### Description

The `StreamRoom` component creates a context for a specific streaming room, handling:

- Stream metadata and state management
- WebSocket room connection and event handling
- Guest request management
- Token generation and authentication
- User type management (host, co-host, guest)

`StreamRoom` must be placed inside `VidbloqProvider` but can be used multiple times in your application for different rooms.