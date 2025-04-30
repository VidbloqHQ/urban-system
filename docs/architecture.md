# SDK Architecture Guide

This guide explains the architecture of the Vidbloq SDK, helping developers understand how the different components work together.

## Overview

The Vidbloq SDK follows a context-based architecture using React's Context API. The SDK is organized into several layers:

1. **Provider Layer** - Context providers that manage global state
2. **Hook Layer** - Custom hooks that expose functionality to components
3. **Component Layer** - Ready-to-use UI components
4. **Utility Layer** - Helper functions and API clients

## Provider Layer

The provider layer consists of several context providers that manage different aspects of the application state:

### Provider Hierarchy

```
VidbloqProvider
├── TenantProvider (API authentication and tenant information)
│   ├── ThemeProvider (UI theming)
│   │   ├── WalletProvider (Wallet connection and transactions)
│   │   │   └── NotificationProvider (User notifications)
│   │   │
└── StreamRoom
    └── StreamProvider (Stream-specific state)
        └── VidbloqProgramProvider (Blockchain program interactions)
```

Each provider encapsulates a specific aspect of functionality and makes it available to the components below it in the tree.

### TenantProvider

The `TenantProvider` is responsible for:

- API authentication using your API key and secret
- WebSocket connection establishment and maintenance
- Storing tenant-level information

### WalletProvider

The `WalletProvider` manages wallet connections and transactions:

- Storing the user's public key
- Handling wallet connection/disconnection
- Providing transaction signing capabilities
- Persisting wallet state across sessions

### StreamProvider

The `StreamProvider` handles stream-specific state:

- Stream metadata and settings
- User roles (host, co-host, guest)
- Guest request management
- Stream tokens and authentication
- Real-time communication related to the specific stream

### NotificationProvider

The `NotificationProvider` manages the notification system:

- Creating, displaying, and removing notifications
- Managing notification timeouts
- Providing different notification types (success, error, info, warning)

## Hook Layer

The hooks layer provides access to the context state and adds additional functionality:

### Context Access Hooks

- `useTenantContext` - Access tenant information and API client
- `useStreamContext` - Access stream state and functions
- `useWalletContext` - Access wallet connection and signing functions
- `useNotification` - Access notification system

### Functional Hooks

- `usePrejoin` - Manage pre-join experience with camera and microphone setup
- `useLivestream` - Optimize participant display for livestream format
- `useMeeting` - Optimize participant display for meeting format
- `useWebSocket` - Low-level WebSocket communication
- `useCreateStream` - Stream creation functionality
- `useTransaction` - Payment and transaction handling
- `useRequirePublicKey` - Enforce wallet connection

## Component Layer

The component layer consists of ready-to-use UI components:

### Container Components

- `VidbloqProvider` - Root SDK provider
- `StreamRoom` - Stream room container
- `StreamView` - Video display container

### Media Components

- `MicrophoneControl` - Toggle microphone
- `CameraControl` - Toggle camera
- `ScreenShareControl` - Toggle screen sharing
- `RecordControl` - Toggle recording
- `MediaControls` - Combined media controls

### Integration Components

- `WalletAdapterBridge` - Bridge between Solana wallet adapter and SDK wallet context
- `BaseCallControls` - Headless component for call controls

## Utility Layer

The utility layer provides various helper functions and classes:

- `ApiClient` - HTTP client for API calls
- Wallet utilities for registration and management
- WebSocket communication utilities
- Type definitions

## Data Flow

### Authentication Flow

```
1. VidbloqProvider initializes with API key and secret
2. TenantProvider creates ApiClient with credentials
3. ApiClient is used for authenticated API calls
4. WebSocket connection is established with credentials
5. Tenant information is fetched and stored in context
```

### Stream Join Flow

```
1. User enters prejoin screen
2. usePrejoin hook initializes camera and microphone
3. User enters nickname and joins stream
4. Token is generated using API client
5. LiveKit room is connected with token
6. WebSocket joins the room channel
7. Stream view is rendered with participants
```

### WebSocket Event Flow

```
1. WebSocket receives event from server
2. Event is dispatched to registered listeners
3. Context state is updated based on event
4. Components re-render with new state
5. UI reflects the updated state
```

## Key Concepts

### Context-Based State Management

The SDK uses React's Context API for state management, allowing components to access shared state without prop drilling. Each context is focused on a specific domain:

- `TenantContext` - Tenant-level state
- `WalletContext` - Wallet connection state
- `StreamContext` - Stream-specific state
- `NotificationContext` - Notification state

### Hooks for Functional Logic

Hooks encapsulate complex logic and provide a clean interface for components. They follow these patterns:

- Context access hooks (`useContext` wrappers)
- Stateful hooks (managing internal state)
- Effect hooks (side effects and lifecycle)
- Callback hooks (memoized functions)

### Component Composition

Components are designed to be composed together to create complex UIs:

```jsx
<StreamRoom roomName="my-room">
  <div className="custom-layout">
    <StreamView />
    <div className="controls">
      <MediaControls />
      <CustomButton onClick={handleSomeAction} />
    </div>
  </div>
</StreamRoom>
```

### Headless Components

Some components like `BaseCallControls` are "headless" - they provide functionality without UI, allowing complete customization:

```jsx
<BaseCallControls
  render={props => <CustomUIComponent {...props} />}
/>
```

## Implementation Details

### WebSocket Communication

The SDK uses a custom WebSocket implementation with:

- Automatic reconnection
- Event-based message handling
- Room-based channel subscription
- Authentication handling

### LiveKit Integration

Video streaming is powered by LiveKit, which provides:

- Real-time video and audio streaming
- Screen sharing
- Track management
- Room participation

### Wallet Integration

Wallet functionality includes:

- Support for Solana wallets
- Bridge to Solana wallet adapter
- Transaction signing
- Wallet state persistence

## Extending the SDK

### Creating Custom UI

You can create custom UI components using the SDK's hooks:

```jsx
function CustomStreamUI() {
  const { userType, streamMetadata } = useStreamContext();
  const { participants } = useLivestream();
  
  return (
    <div className="my-custom-ui">
      <h1>{streamMetadata.title}</h1>
      <p>User type: {userType}</p>
      
      {/* Custom participant rendering */}
      <div className="participants-grid">
        {participants.map(participant => (
          <CustomParticipantView 
            key={participant.identity}
            participant={participant}
          />
        ))}
      </div>
      
      {/* Custom controls */}
      <div className="custom-controls">
        <CustomMicButton />
        <CustomCameraButton />
      </div>
    </div>
  );
}
```

### Adding Custom WebSocket Events

You can listen for and send custom WebSocket events:

```jsx
function CustomEventHandler() {
  const { websocket } = useStreamContext();
  
  useEffect(() => {
    // Add listener for custom event
    websocket.addEventListener('customEvent', handleCustomEvent);
    
    return () => {
      // Remove listener when component unmounts
      websocket.removeEventListener('customEvent', handleCustomEvent);
    };
  }, [websocket]);
  
  const handleCustomEvent = (data) => {
    console.log('Custom event received:', data);
  };
  
  const sendCustomEvent = () => {
    websocket.sendMessage('customEvent', { foo: 'bar' });
  };
  
  return <button onClick={sendCustomEvent}>Send Custom Event</button>;
}
```

### Implementing Custom Authentication

You can implement custom authentication flows:

```jsx
function CustomAuthFlow() {
  const { apiClient } = useTenantContext();
  const { generateToken } = useStreamContext();
  
  const handleCustomAuth = async () => {
    // Get custom auth data
    const authData = await apiClient.post('/custom-auth', {
      // Custom auth parameters
    });
    
    // Use the auth data to generate a token
    await generateToken(authData.username);
  };
  
  return <button onClick={handleCustomAuth}>Custom Auth</button>;
}
```

## Best Practices

### Provider Placement

-