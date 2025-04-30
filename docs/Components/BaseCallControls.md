### BaseCallControls

A headless component that provides all call control functionality without UI.

```jsx
import { BaseCallControls } from '@vidbloq/react';

function MyCustomControls() {
  return (
    <BaseCallControls
      onDisconnect={() => console.log('Disconnected')}
      onRaiseHand={() => console.log('Hand raised')}
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
            <div className="media-controls">
              <button onClick={toggleMic}>
                {isMicEnabled ? 'Mute' : 'Unmute'}
              </button>
              <button onClick={toggleCamera}>
                {isCameraEnabled ? 'Hide Camera' : 'Show Camera'}
              </button>
              <button onClick={toggleScreenShare}>
                {isScreenSharingEnabled ? 'Stop Sharing' : 'Share Screen'}
              </button>
            </div>
          )}
          
          {isGuest && !isInvited && !hasPendingRequest && (
            <button onClick={requestToSpeak}>Raise Hand</button>
          )}
          
          {userType === "host" && (
            <button onClick={toggleRecording}>
              {isRecording ? 'Stop Recording' : 'Start Recording'}
            </button>
          )}
          
          <button onClick={handleDisconnectClick}>Leave Stream</button>
        </div>
      )}
    />
  );
}
```

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
| customHandlers | Record string () => void | Custom event handlers |
| render | (props: CallControlsRenderProps) => React.ReactNode | Render prop for custom UI - if provided, the component becomes headless and lets you render a completely custom UI |

#### CallControlsRenderProps

| Prop | Type | Description |
|------|------|-------------|
| isInvited | boolean | Whether the current user has been invited to speak |
| hasPendingRequest | boolean | Whether the current user has a pending request to speak |
| canAccessMediaControls | boolean | Whether the user can access media controls |
| isGuest | boolean | Whether the user is a guest |
| isMicEnabled | boolean | Whether the microphone is enabled |
| isCameraEnabled | boolean | Whether the camera is enabled |
| isScreenSharingEnabled | boolean | Whether screen sharing is enabled |
| isRecording | boolean | Whether recording is active |
| handleDisconnectClick | () => Promise void | Function to disconnect from the call |
| toggleMic | () => void | Function to toggle microphone |
| toggleCamera | () => void | Function to toggle camera |
| toggleScreenShare | () => void | Function to toggle screen sharing |
| toggleRecording | () => void | Function to toggle recording |
| requestToSpeak | () => void | Function to request to speak (for guests) |
| userType | "host" \| "co-host" \| "guest" \| null | Current user type in the stream |