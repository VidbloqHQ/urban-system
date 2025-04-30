# Vidbloq SDK Documentation

## Introduction

Vidbloq SDK is a comprehensive toolkit for integrating real-time video streaming and communication features into your React applications. The SDK provides ready-to-use components and hooks for creating interactive streaming experiences with features like:

- Video and audio streaming
- Screen sharing
- Host/guest management
- Wallet integration for Web3 applications
- Chat and reaction capabilities
- And more

## Getting Started

### Installation

```bash
npm install @vidbloq/react
# or
yarn add @vidbloq/react
```

### Basic Setup

The entry point to using the SDK is the `VidbloqProvider` component. This component initializes the necessary contexts and connections for your application.

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

### Using with Solana Wallet Adapter

If you're using Solana wallets in your application:

```jsx
import { WalletAdapterBridge, VidbloqProvider } from '@vidbloq/react';
import { WalletProvider } from '@solana/wallet-adapter-react';
// Import wallet adapters and other components

function App() {
  return (
    <WalletProvider wallets={[/* your wallet adapters */]}>
      <VidbloqProvider apiKey="YOUR_API_KEY" apiSecret="YOUR_API_SECRET">
        <WalletAdapterBridge />
        {/* Your application components */}
      </VidbloqProvider>
    </WalletProvider>
  );
}
```

## Core Components

### VidbloqProvider

The root component that initializes the SDK and provides global context.

#### Props

| Prop | Type | Required | Description |
|------|------|----------|-------------|
| apiKey | string | Yes | Your Vidbloq API key |
| apiSecret | string | Yes | Your Vidbloq API secret |
| websocketUrl | string | No | Custom WebSocket URL (optional) |
| children | React.ReactNode | Yes | Child components |

### StreamRoom

The component that creates a stream room context for hosting or joining a stream.

#### Props

| Prop | Type | Required | Description |
|------|------|----------|-------------|
| roomName | string | Yes | Unique identifier for the stream room |
| children | React.ReactNode | Yes | Child components |

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

### StreamView

The component that renders the actual stream view with participants.

#### Props

| Prop | Type | Required | Description |
|------|------|----------|-------------|
| children | React.ReactNode | No | Optional custom UI components |

### Prejoin

A component for the pre-join experience where users can set up their camera, microphone, and display name before joining a stream.

### Media Control Components

The SDK provides individual components for controlling media:

- **MicrophoneControl**: Toggle microphone on/off
- **CameraControl**: Toggle camera on/off
- **ScreenShareControl**: Toggle screen sharing
- **RecordControl**: Start/stop recording
- **MediaControls**: A combined component that includes mic, camera, and screen sharing controls

#### Example

```jsx
import { MediaControls } from '@vidbloq/react';

function MyCallUI() {
  return (
    <div>
      <MediaControls 
        showLabels={true}
        onChange={{
          mic: (enabled) => console.log('Mic:', enabled),
          camera: (enabled) => console.log('Camera:', enabled),
          screenShare: (enabled) => console.log('Screen share:', enabled),
        }}
      />
    </div>
  );
}
```

### BaseCallControls

A headless component (no UI) that provides all the call control functionality and can be customized with your own UI.

#### Props

| Prop | Type | Description |
|------|------|-------------|
| onRaiseHand | () => void | Callback when a guest raises their hand |
| onReturnToGuest | () => void | Callback when a temporary host is returned to guest status |
| onDisconnect | () => void | Callback when disconnecting from the call |
| onAgendaToggle | () => void | Callback for toggling agenda view |
| onChatToggle | () => void | Callback for toggling chat view |
| onReactionsToggle | () => void | Callback for toggling reactions panel |
| onRecordToggle | () => void | Callback for toggling recording |
| customHandlers | Record string() => void | Custom event handlers |
| render | (props: CallControlsRenderProps) => React.ReactNode | Render prop for custom UI |

#### Example with Render Props

```jsx
import { BaseCallControls } from '@vidbloq/react';

function MyCustomControls() {
  return (
    <BaseCallControls
      onDisconnect={() => console.log('Disconnected')}
      render={({
        isInvited,
        hasPendingRequest,
        canAccessMediaControls,
        isGuest,
        isMicEnabled,
        isCameraEnabled,
        isScreenSharingEnabled,
        isRecording,
        toggleMic,
        toggleCamera,
        toggleScreenShare,
        toggleRecording,
        requestToSpeak,
        handleDisconnectClick,
        userType,
      }) => (
        <div className="my-custom-controls">
          {canAccessMediaControls && (
            <>
              <button onClick={toggleMic}>
                {isMicEnabled ? 'Mute' : 'Unmute'}
              </button>
              <button onClick={toggleCamera}>
                {isCameraEnabled ? 'Hide Camera' : 'Show Camera'}
              </button>
            </>
          )}
          
          {isGuest && !hasPendingRequest && !isInvited && (
            <button onClick={requestToSpeak}>Raise Hand</button>
          )}
          
          <button onClick={handleDisconnectClick}>Leave</button>
        </div>
      )}
    />
  );
}
```

### WalletAdapterBridge

Bridges the Solana wallet adapter with the SDK's internal wallet context.

## Hooks

The SDK provides a rich set of hooks that give you access to various functionalities.

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
  
  // Use tenant information
  
  return <div>{tenant?.name}</div>;
}
```

### useStreamContext

Provides access to the current stream context with information about the stream and participants.

```jsx
import { useStreamContext } from '@vidbloq/react';

function MyStreamComponent() {
  const { 
    roomName, 
    userType, 
    websocket, 
    streamMetadata, 
    guestRequests, 
    generateToken 
  } = useStreamContext();
  
  // Use stream information
  
  return <div>Room: {roomName}</div>;
}
```

### useWalletContext

Provides access to the connected wallet and signing functionality.

```jsx
import { useWalletContext } from '@vidbloq/react';

function MyWalletComponent() {
  const { 
    publicKey, 
    connectWallet, 
    clearWallet, 
    signTransaction, 
    connected 
  } = useWalletContext();
  
  return (
    <div>
      {publicKey ? 
        <p>Connected: {publicKey.toString()}</p> : 
        <button onClick={() => /* connect wallet */}>Connect Wallet</button>
      }
    </div>
  );
}
```

### useRequirePublicKey

A utility hook that ensures a public key is available, with optional signing capability requirement.

```jsx
import { useRequirePublicKey } from '@vidbloq/react';

function MyComponent() {
  // Will prompt the user to connect their wallet if not connected
  const { publicKey, hasSigningCapability } = useRequirePublicKey(true); // true = require signing capability
  
  // Rest of component
}
```

### usePrejoin

Manages the pre-join experience for streams, including camera/mic setup and nickname.

```jsx
import { usePrejoin } from '@vidbloq/react';

function MyPrejoinComponent() {
  const { 
    nickname, 
    setNickname, 
    previewTracks, 
    videoRef, 
    handleAudioToggle, 
    handleVideoToggle, 
    canControlMedia, 
    isLoading, 
    joinStream, 
    error 
  } = usePrejoin({ 
    initialNickname: 'Guest', 
    publicKey: myPublicKey 
  });
  
  return (
    <div>
      <input
        type="text"
        value={nickname}
        onChange={(e) => setNickname(e.target.value)}
        placeholder="Your name"
      />
      
      <video ref={videoRef} autoPlay muted />
      
      {canControlMedia && (
        <>
          <button onClick={() => handleAudioToggle(true)}>Enable Mic</button>
          <button onClick={() => handleVideoToggle(true)}>Enable Camera</button>
        </>
      )}
      
      <button onClick={joinStream} disabled={isLoading}>
        {isLoading ? 'Joining...' : 'Join Stream'}
      </button>
      
      {error && <p>Error: {error.message}</p>}
    </div>
  );
}
```

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

### useLivestream

Provides access to livestream functionality with participant management.

```jsx
import { useLivestream } from '@vidbloq/react';

function LivestreamComponent() {
  const { 
    rawTracks, 
    activeSpeaker, 
    screenShareTrack, 
    screenSharerIdentity, 
    participantsByType, 
    mainContent, 
    sidebarContent, 
    room 
  } = useLivestream();
  
  // Use this data to build a custom livestream UI
}
```

### useMeeting

Provides access to meeting functionality with optimized layout and speaker detection.

```jsx
import { useMeeting } from '@vidbloq/react';

function MeetingComponent() {
  const { 
    room, 
    activeSpeaker, 
    cameraTracks, 
    screenShareTrack, 
    hostTrack, 
    coHostTracks, 
    displayedCoHosts, 
    getBottomRowParticipants, 
    calculateLayoutType 
  } = useMeeting();
  
  // Use this data to build a custom meeting UI
}
```

### useTransaction

Facilitates cryptocurrency transactions using the connected wallet.

```jsx
import { useTransaction, useWalletContext } from '@vidbloq/react';
import { PublicKey } from '@solana/web3.js';

function TransactionComponent() {
  const { publicKey } = useWalletContext();
  
  const recipients = [
    {
      publicKey: new PublicKey('recipient-address'),
      amount: 0.1 // SOL amount
    }
  ];
  
  const { 
    fetchTransaction, 
    signAndSubmitTransaction, 
    transactionBase64, 
    transactionSignature, 
    error, 
    loading 
  } = useTransaction({
    recipients,
    tokenName: 'sol'
  });
  
  const handleSendTransaction = async () => {
    try {
      await fetchTransaction();
      await signAndSubmitTransaction();
      console.log('Transaction successful:', transactionSignature);
    } catch (err) {
      console.error('Transaction failed:', err);
    }
  };
  
  return (
    <div>
      <button onClick={handleSendTransaction} disabled={loading || !publicKey}>
        {loading ? 'Processing...' : 'Send SOL'}
      </button>
      
      {error && <p>Error: {error}</p>}
      {transactionSignature && <p>Transaction complete!</p>}
    </div>
  );
}
```

### useWebSocket

A low-level hook for WebSocket communication. Most applications will use higher-level hooks and not interact with this directly.

## Advanced Usage

### Creating a Custom UI

The SDK is designed to be flexible, allowing you to create custom UIs by using the hooks and headless components.

```jsx
import { 
  StreamRoom, 
  useMeeting, 
  BaseCallControls 
} from '@vidbloq/react';

function CustomStreamUI() {
  const {
    cameraTracks,
    screenShareTrack,
    activeSpeaker
  } = useMeeting();
  
  // Implement your custom UI using the data from the hook
  
  return (
    <div className="custom-stream-layout">
      {/* Main video area */}
      <div className="main-video">
        {screenShareTrack ? (
          <ScreenShareRenderer track={screenShareTrack} />
        ) : (
          <ParticipantRenderer track={cameraTracks[0]} />
        )}
      </div>
      
      {/* Participants grid */}
      <div className="participants-grid">
        {cameraTracks.map(track => (
          <ParticipantThumbnail 
            key={track.participant.identity}
            track={track}
            isActive={activeSpeaker === track.participant.identity}
          />
        ))}
      </div>
      
      {/* Custom controls */}
      <BaseCallControls
        render={controlProps => (
          <CustomControlsUI {...controlProps} />
        )}
      />
    </div>
  );
}

function StreamWithCustomUI() {
  return (
    <StreamRoom roomName="custom-ui-room">
      <CustomStreamUI />
    </StreamRoom>
  );
}
```

### Managing Guest Requests

Host applications can handle guest requests to speak:

```jsx
import { useStreamContext } from '@vidbloq/react';

function GuestRequestsManager() {
  const { guestRequests, websocket, roomName } = useStreamContext();
  
  const handleInviteGuest = (participantId) => {
    websocket.inviteGuest(roomName, participantId);
  };
  
  const handleReturnToGuest = (participantId) => {
    websocket.returnToGuest(roomName, participantId);
  };
  
  return (
    <div className="guest-requests">
      <h3>Pending Requests ({guestRequests.length})</h3>
      <ul>
        {guestRequests.map(request => (
          <li key={request.participantId}>
            {request.name}
            <button onClick={() => handleInviteGuest(request.participantId)}>
              Invite to Speak
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

## API Reference

Comprehensive reference for all components, hooks, and utilities provided by the SDK.

### Components
- VidbloqProvider
- StreamRoom
- StreamView
- WalletAdapterBridge
- BaseCallControls
- MicrophoneControl
- CameraControl
- ScreenShareControl
- RecordControl
- MediaControls
- UserView
- Prejoin

### Hooks
- useTenantContext
- useStreamContext
- useWalletContext
- useRequirePublicKey
- usePrejoin
- useCreateStream
- useLivestream
- useMeeting
- useTransaction
- useWebSocket
- useParticipantList
- useVidbloqProgram

## Troubleshooting

Common issues and their solutions:

### WebSocket Connection Issues

If you're experiencing WebSocket connection issues:

1. Ensure your API key and secret are correct
2. Check if your network allows WebSocket connections
3. Verify the WebSocket URL is accessible

### Media Device Issues

If camera or microphone aren't working:

1. Ensure the browser has necessary permissions
2. Check if another application is using the devices
3. Verify the devices are properly connected and working

### Wallet Connection Issues

If wallet connection is failing:

1. Ensure the wallet adapter is properly initialized
2. Check that the wallet extension is installed and unlocked
3. Verify you're connecting to the correct network

## Migration Guide

### Migrating from v1.x to v2.x

Key changes:

1. The `StreamProvider` is now wrapped by `StreamRoom`
2. WebSocket connection handling has been improved
3. New hooks have been added for easier customization

Update your imports:

```jsx
// Old v1.x
import { StreamProvider } from '@vidbloq/react';

// New v2.x
import { StreamRoom } from '@vidbloq/react';
```

Update your component hierarchy:

```jsx
// Old v1.x
<StreamProvider roomName="my-room">
  <YourComponents />
</StreamProvider>

// New v2.x
<StreamRoom roomName="my-room">
  <YourComponents />
</StreamRoom>
```

## FAQ

**Q: Can I use the SDK with Next.js?**  
A: Yes, the SDK is compatible with Next.js. For server-side rendering, use dynamic imports for components that access browser APIs.

**Q: Does the SDK support mobile browsers?**  
A: Yes, the SDK works on modern mobile browsers that support WebRTC.

**Q: How many participants can join a stream?**  
A: The number of participants depends on your plan. Basic plans support up to 50 viewers and 10 active participants.

**Q: Is there a way to record streams?**  
A: Yes, hosts can record streams using the RecordControl component or the API.

**Q: Can I customize the UI completely?**  
A: Yes, the SDK provides headless components and hooks that allow you to build completely custom UIs.

## Support

For additional help, please contact support@vidbloq.com or visit our [support center](https://support.vidbloq.com).