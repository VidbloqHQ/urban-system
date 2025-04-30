### useTenantContext

Provides access to the tenant context with information about the current tenant and API client.

```jsx
import { useTenantContext } from '@vidbloq/react';

function MyComponent() {
  const { 
    apiKey, 
    apiSecret, 
    websocket, 
    isConnected, 
    connect, 
    disconnect, 
    apiClient, 
    tenant, 
    isLoading 
  } = useTenantContext();
  
  // Use tenant information and API client
  
  return <div>{tenant?.name}</div>;
}
```

#### Return Value

| Property | Type | Description |
|----------|------|-------------|
| apiKey | string | Your Vidbloq API key |
| apiSecret | string | Your Vidbloq API secret |
| websocket | ReturnType typeof useWebSocket | WebSocket instance for real-time communication |
| isConnected | boolean | Whether WebSocket is connected |
| connect | () => Promise void | Function to connect WebSocket |
| disconnect | () => void | Function to disconnect WebSocket |
| apiClient | ApiClient | HTTP client for making API requests |
| tenant | Tenant \| null | Current tenant information |
| isLoading | boolean | Whether tenant data is loading |

#### Description

The `useTenantContext` hook provides access to the tenant context created by the `TenantProvider` component. This hook is essential for accessing API functionality and WebSocket connections. It throws an error if used outside a `TenantProvider`.