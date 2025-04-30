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

#### Return Value

| Property | Type | Description |
|----------|------|-------------|
| roomName | string | Current room name |
| userType | "host" \| "co-host" \| "guest" \| null | User's role in the stream |
| websocket | ReturnType typeof useWebSocket \| null | WebSocket instance for the stream |
| streamMetadata | StreamMetadata | Metadata about the stream (title, type, etc.) |
| setStreamMetadata | (metadata: StreamMetadata) => void | Function to update stream metadata |
| guestRequests | GuestRequest[] | List of pending guest requests to speak |
| setGuestRequests | (requests: GuestRequest[]) => void | Function to update guest requests |
| identity | string \| undefined | User's identity in the stream |
| setIdentity | (identity: string) => void | Function to set user identity |
| showAgendaModal | boolean | Whether to show agenda modal |
| setShowAgendaModal | (show: boolean) => void | Function to toggle agenda modal |
| showAddonModal | boolean | Whether to show addon modal |
| setShowAddonModal | (show: boolean) => void | Function to toggle addon modal |
| showTransactionModal | boolean | Whether to show transaction modal |
| setShowTransactionModal | (show: boolean) => void | Function to toggle transaction modal |
| token | string \| undefined | Auth token for the stream |
| generateToken | (val: string) => Promise void | Function to generate a new token |
| setToken | (token: string \| undefined) => void | Function to set token directly |
| agendas | Agenda[] | List of agendas for the stream |
| setAgendas | (agendas: Agenda[]) => void | Function to update agendas |
| currentTime | number | Current stream time in seconds |
| setCurrentTime | (val: number) => void | Function to update current time |
| audioEnabled | boolean | Whether audio is enabled |
| setAudioEnabled | (val: boolean) => void | Function to toggle audio |
| videoEnabled | boolean | Whether video is enabled |
| setVideoEnabled | (val: boolean) => void | Function to toggle video |

#### Description

The `useStreamContext` hook provides access to the stream context created by the `StreamProvider` component. This hook is used to interact with the current stream session, manage participants, and handle stream-specific settings. It throws an error if used outside a `StreamProvider`.