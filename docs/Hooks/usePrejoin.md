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

#### Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| options | UsePrejoinOptions | No | {} | Configuration options |

#### UsePrejoinOptions

| Property | Type | Required | Default | Description |
|----------|------|----------|---------|-------------|
| initialNickname | string | No | '' | Initial value for nickname |
| publicKey |  toString: () => string; toBase58: () => string  \| null | No | null | User's wallet public key |

#### Return Value

| Property | Type | Description |
|----------|------|-------------|
| nickname | string | Current nickname value |
| setNickname | React.Dispatch React.SetStateActionstring | Function to update nickname |
| previewTracks | PreviewTrackState | Current preview tracks state |
| videoRef | React.RefObject HTMLVideoElement | Ref to attach to video element |
| handleAudioToggle | (enabled: boolean) => void | Function to toggle audio track |
| handleVideoToggle | (enabled: boolean) => void | Function to toggle video track |
| canControlMedia | boolean | Whether user has permission to control media |
| isLoading | boolean | Whether token generation is in progress |
| joinStream | () => Promise void | Function to join the stream |
| error | Error \| null | Any error that occurred |

#### Description

The `usePrejoin` hook manages the pre-join experience for a stream, including:

- Camera and microphone initialization
- Video preview
- Nickname management
- Permission handling based on stream type
- Stream joining process and token generation

This hook is typically used in a prejoin screen component before entering the main stream view.