### VidbloqProvider

The root component that initializes the SDK and provides global context.

```jsx
import { VidbloqProvider } from '@vidbloq/react';

function App() {
  return (
    <VidbloqProvider 
      apiKey="YOUR_API_KEY" 
      apiSecret="YOUR_API_SECRET"
    >
      {/* Your application components */}
    </VidbloqProvider>
  );
}
```

#### Props

| Prop | Type | Required | Description |
|------|------|----------|-------------|
| apiKey | string | Yes | Your Vidbloq API key |
| apiSecret | string | Yes | Your Vidbloq API secret |
| websocketUrl | string | No | Custom WebSocket URL (optional) |
| children | React.ReactNode | Yes | Child components |

#### Description

The `VidbloqProvider` component is responsible for:

- Initializing the API client with your credentials
- Establishing and maintaining WebSocket connections
- Setting up the wallet context for blockchain interactions
- Providing notification capabilities
- Applying theme styling

This component must be placed at the root of your application, wrapping all other Vidbloq components.

#### Example with Custom WebSocket URL

```jsx
<VidbloqProvider 
  apiKey="YOUR_API_KEY" 
  apiSecret="YOUR_API_SECRET"
  websocketUrl="wss://custom-websocket-server.example.com"
>
  {/* Your application components */}
</VidbloqProvider>
```
