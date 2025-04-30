### UserView

A component that renders a participant view optimized for different stream types.

```jsx
import { StreamRoom, StreamView, UserView } from '@vidbloq/react';

function MyStreamRoom() {
  return (
    <StreamRoom roomName="my-awesome-stream">
      <StreamView>
        <UserView />
      </StreamView>
    </StreamRoom>
  );
}
```

#### Description

The `UserView` component automatically handles:

- Rendering participants based on user type (host, co-host, guest)
- Screen sharing views
- Active speaker detection
- Positioning of video elements
- Responsive layout based on device size

This component is the default view inside `StreamView` but can be customized or replaced with your own implementation.