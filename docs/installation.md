# Installation and Setup Guide

This guide will walk you through installing and setting up the Vidbloq SDK in your application.

## Prerequisites

Before installing the Vidbloq SDK, make sure you have:

- Node.js (v14 or later)
- npm or yarn
- A Vidbloq account with API credentials
- React (v16.8 or later) installed in your project

## Installation

### 1. Install the SDK

Using npm:

```bash
npm install @vidbloq/react
```

Or using yarn:

```bash
yarn add @vidbloq/react
```

### 2. Install Peer Dependencies

The Vidbloq SDK has several peer dependencies that you'll need to install if you don't already have them in your project:

```bash
npm install react react-dom @solana/web3.js buffer
```

If you plan to use the wallet integration features, you'll also need:

```bash
npm install @solana/wallet-adapter-react
```

## Basic Setup

### 1. Set Up the VidbloqProvider

Create a wrapper component or modify your main app component to include the `VidbloqProvider`:

```jsx
// App.jsx or App.tsx
import React from 'react';
import { VidbloqProvider } from '@vidbloq/react';

function App() {
  return (
    <VidbloqProvider 
      apiKey="YOUR_API_KEY" 
      apiSecret="YOUR_API_SECRET"
    >
      {/* Your application components */}
      <YourComponents />
    </VidbloqProvider>
  );
}

export default App;
```

Replace `YOUR_API_KEY` and `YOUR_API_SECRET` with your actual Vidbloq API credentials.

### 2. Accessing SDK Functionality

Once the provider is set up, you can use the SDK's hooks and components throughout your application:

```jsx
import { useStreamContext, MediaControls } from '@vidbloq/react';

function YourComponent() {
  const { roomName, userType } = useStreamContext();
  
  return (
    <div>
      <h1>Room: {roomName}</h1>
      <p>User Type: {userType || 'Not connected'}</p>
      
      <MediaControls showLabels={true} />
    </div>
  );
}
```

## Wallet Integration

### 1. Set Up Solana Wallet Adapter

If you're using Solana wallets, set up the wallet adapter components:

```jsx
import React, { useMemo } from 'react';
import { WalletAdapterNetwork } from '@solana/wallet-adapter-base';
import { WalletProvider, ConnectionProvider } from '@solana/wallet-adapter-react';
import { PhantomWalletAdapter, SolflareWalletAdapter } from '@solana/wallet-adapter-wallets';
import { WalletModalProvider } from '@solana/wallet-adapter-react-ui';
import { clusterApiUrl } from '@solana/web3.js';
import { VidbloqProvider, WalletAdapterBridge } from '@vidbloq/react';

// Import the wallet adapter styles
import '@solana/wallet-adapter-react-ui/styles.css';

function App() {
  // Set up Solana network and wallets
  const network = WalletAdapterNetwork.Mainnet;
  const endpoint = useMemo(() => clusterApiUrl(network), [network]);
  
  const wallets = useMemo(
    () => [
      new PhantomWalletAdapter(),
      new SolflareWalletAdapter(),
      // Add more wallet adapters as needed
    ],
    []
  );

  return (
    <ConnectionProvider endpoint={endpoint}>
      <WalletProvider wallets={wallets} autoConnect>
        <WalletModalProvider>
          <VidbloqProvider 
            apiKey="YOUR_API_KEY" 
            apiSecret="YOUR_API_SECRET"
          >
            {/* Include the bridge component to connect wallet adapter */}
            <WalletAdapterBridge />
            
            {/* Your application components */}
            <YourComponents />
          </VidbloqProvider>
        </WalletModalProvider>
      </WalletProvider>
    </ConnectionProvider>
  );
}

export default App;
```

## Creating a Stream Room

To create a stream room using the SDK:

```jsx
import React from 'react';
import { StreamRoom, StreamView, MediaControls } from '@vidbloq/react';

function StreamPage() {
  const roomName = 'my-awesome-stream'; // Get this from URL, state, or props
  
  return (
    <StreamRoom roomName={roomName}>
      <div className="stream-container">
        <StreamView />
        
        <div className="controls-container">
          <MediaControls showLabels={true} />
        </div>
      </div>
    </StreamRoom>
  );
}
```

## Pre-join Experience

Create a pre-join experience to allow users to set up their camera, microphone, and display name:

```jsx
import React from 'react';
import { useStreamContext, usePrejoin } from '@vidbloq/react';

function PrejoinScreen() {
  const { roomName } = useStreamContext();
  const { 
    nickname, 
    setNickname, 
    videoRef, 
    handleAudioToggle, 
    handleVideoToggle, 
    joinStream 
  } = usePrejoin({ initialNickname: 'Guest User' });
  
  return (
    <div className="prejoin-container">
      <h1>Join Stream: {roomName}</h1>
      
      <div className="video-preview">
        <video ref={videoRef} autoPlay playsInline muted />
      </div>
      
      <div className="settings">
        <input
          type="text"
          value={nickname}
          onChange={(e) => setNickname(e.target.value)}
          placeholder="Your name"
        />
        
        <div className="media-toggles">
          <button onClick={() => handleAudioToggle(true)}>Enable Mic</button>
          <button onClick={() => handleVideoToggle(true)}>Enable Camera</button>
        </div>
        
        <button 
          className="join-button"
          onClick={joinStream}
        >
          Join Stream
        </button>
      </div>
    </div>
  );
}
```

## Complete Application Example

Here's a complete example that puts everything together:

```jsx
import React, { useState } from 'react';
import { 
  VidbloqProvider, 
  StreamRoom, 
  StreamView, 
  MediaControls,
  BaseCallControls,
  usePrejoin
} from '@vidbloq/react';

// Main App component
function App() {
  const [apiKey] = useState('YOUR_API_KEY');
  const [apiSecret] = useState('YOUR_API_SECRET');
  const [roomName] = useState('demo-stream-room');
  
  return (
    <VidbloqProvider apiKey={apiKey} apiSecret={apiSecret}>
      <StreamRoom roomName={roomName}>
        <StreamApp />
      </StreamRoom>
    </VidbloqProvider>
  );
}

// Stream application with prejoin and stream view
function StreamApp() {
  const [hasJoined, setHasJoined] = useState(false);
  
  // Conditionally render prejoin or stream view
  if (!hasJoined) {
    return <PrejoinScreen onJoin={() => setHasJoined(true)} />;
  }
  
  return (
    <div className="stream-container">
      <StreamView />
      <ControlsPanel />
    </div>
  );
}

// Prejoin screen component
function PrejoinScreen({ onJoin }) {
  const { 
    nickname, 
    setNickname, 
    videoRef, 
    handleAudioToggle, 
    handleVideoToggle, 
    joinStream,
    isLoading 
  } = usePrejoin();
  
  const handleJoin = async () => {
    await joinStream();
    onJoin();
  };
  
  return (
    <div className="prejoin-screen">
      <div className="video-preview">
        <video ref={videoRef} autoPlay playsInline muted />
      </div>
      
      <div className="settings">
        <input
          type="text"
          value={nickname}
          onChange={(e) => setNickname(e.target.value)}
          placeholder="Your name"
        />
        
        <div className="media-toggles">
          <button onClick={() => handleAudioToggle(true)}>Enable Mic</button>
          <button onClick={() => handleVideoToggle(true)}>Enable Camera</button>
        </div>
        
        <button 
          onClick={handleJoin}
          disabled={isLoading || !nickname.trim()}
        >
          {isLoading ? 'Joining...' : 'Join Stream'}
        </button>
      </div>
    </div>
  );
}

// Controls panel component
function ControlsPanel() {
  return (
    <div className="controls-panel">
      <MediaControls showLabels={true} />
      
      <BaseCallControls
        onDisconnect={() => console.log('Disconnected')}
        render={({
          isGuest,
          hasPendingRequest,
          requestToSpeak,
          handleDisconnectClick
        }) => (
          <div className="custom-controls">
            {isGuest && !hasPendingRequest && (
              <button onClick={requestToSpeak}>
                Raise Hand
              </button>
            )}
            
            <button 
              className="leave-button"
              onClick={handleDisconnectClick}
            >
              Leave Stream
            </button>
          </div>
        )}
      />
    </div>
  );
}

export default App;
```

## Environment-Specific Setup

### Next.js Setup

When using the SDK with Next.js, you'll need to handle client-side only components:

```jsx
// components/StreamComponent.jsx
'use client';

import { StreamRoom, StreamView } from '@vidbloq/react';

export default function StreamComponent({ roomName }) {
  return (
    <StreamRoom roomName={roomName}>
      <StreamView />
    </StreamRoom>
  );
}
```

Then in your page component:

```jsx
// app/stream/[roomName]/page.jsx
import dynamic from 'next/dynamic';

// Import the component with no SSR
const StreamComponent = dynamic(
  () => import('@/components/StreamComponent'),
  { ssr: false }
);

export default function StreamPage({ params }) {
  const { roomName } = params;
  
  return (
    <main>
      <h1>Stream: {roomName}</h1>
      <StreamComponent roomName={roomName} />
    </main>
  );
}
```

### Styling the SDK

The SDK components come with minimal styling to allow customization. You can either:

1. **Override with CSS:** Target the component classes with your own CSS
2. **Use className prop:** Most components accept a `className` prop for styling
3. **Use render props:** For components like `BaseCallControls`, use the render prop to create fully custom UI

Example with styling:

```jsx
// Custom CSS
import './streamStyles.css';

// Or with Tailwind classes
function MyStyledControls() {
  return (
    <MediaControls 
      className="flex gap-4 p-4 bg-gray-900 rounded-lg"
      showLabels={true}
    />
  );
}
```

## Troubleshooting

### Common Issues

1. **WebSocket Connection Issues**

   If you're having trouble with WebSocket connections:

   ```jsx
   <VidbloqProvider 
     apiKey="YOUR_API_KEY" 
     apiSecret="YOUR_API_SECRET"
     websocketUrl="wss://your-custom-websocket-url.com"
   >
     {/* Your app */}
   </VidbloqProvider>
   ```

2. **Camera/Microphone Access**

   Make sure your application is served over HTTPS in production, as browsers require this for media device access.

3. **Wallet Connection Issues**

   If the wallet adapter bridge isn't working:

   ```jsx
   <WalletProvider wallets={wallets} autoConnect>
     <VidbloqProvider apiKey="YOUR_API_KEY" apiSecret="YOUR_API_SECRET">
       {/* Make sure this is included */}
       <WalletAdapterBridge />
       
       {/* Check connection */}
       <WalletConnectionStatus />
     </VidbloqProvider>
   </WalletProvider>
   
   // Component to debug wallet connection
   function WalletConnectionStatus() {
     const { publicKey, connected } = useWalletContext();
     
     return (
       <div>
         <p>Connected: {connected ? 'Yes' : 'No'}</p>
         <p>Public Key: {publicKey?.toString() || 'None'}</p>
       </div>
     );
   }
   ```

## Next Steps

After completing the basic setup, you can:

1. Explore the [Component Reference](./Components) for detailed component documentation
2. Check out the [Hooks Reference](./Hooks) for available hooks
3. Learn about advanced features like transactions and custom UIs
4. Try creating a complete streaming application with the SDK