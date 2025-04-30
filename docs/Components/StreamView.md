### StreamView

Renders the actual stream view with participants.

```jsx
import { StreamRoom, StreamView } from '@vidbloq/react';

function MyStreamComponent() {
  return (
    <StreamRoom roomName="my-room">
      <StreamView>
        {/* Optional custom UI elements */}
      </StreamView>
    </StreamRoom>
  );
}
```

#### Props

| Prop | Type | Required | Description |
|------|------|----------|-------------|
| children | React.ReactNode | No | Optional custom UI components |

#### Description

The `StreamView` component:

- Initializes the LiveKit room connection
- Handles audio/video publishing permissions based on user type
- Renders the video streams for all participants
- Provides audio rendering for the room

This component must be placed inside a `StreamRoom` component.