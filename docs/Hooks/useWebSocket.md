### useWebSocket

A low-level hook for WebSocket communication.

```jsx
import { useWebSocket } from '@vidbloq/react';

function MyComponent() {
  const ws = useWebSocket({
    url: 'wss://example.com/ws',
    reconnectAttempts: 3,
    reconnectInterval: 5000,
    onOpen: () => console.log('Connected'),
    onClose: () => console.log('Disconnected'),
    onError: (error) => console.error('Error:', error),
  });
  
  // Use WebSocket functionality
  useEffect(() => {
    ws.addEventListener('message', handleMessage);
    return () => ws.removeEventListener('message', handleMessage);
  }, [ws]);
  
  const sendMessage = () => {
    ws.sendMessage('chatMessage', { text: 'Hello!' });
  };
  
  return <button onClick={sendMessage}>Send</button>;
}
```

#### Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| options | WebSocketHookOptions | Yes | - | Configuration options |

#### WebSocketHookOptions

| Property | Type | Required | Default | Description |
|----------|------|----------|---------|-------------|
| url | string | Yes | - | WebSocket server URL |
| reconnectAttempts | number | No | 3 | Number of reconnection attempts |
| reconnectInterval | number | No | 5000 | Milliseconds between reconnection attempts |
| onOpen | (event: Event) => void | No | undefined | Callback when connection opens |
| onClose | (event: CloseEvent) => void | No | undefined | Callback when connection closes |
| onError | (event: Event) => void | No | undefined | Callback when error occurs |

#### Return Value

| Property | Type | Description |
|----------|------|-------------|
| isConnected | boolean | Whether WebSocket is connected |
| lastMessage | WebSocketMessage \| null | Last message received |
| messageHistory | WebSocketMessage[] | History of received messages |
| connect | () => void | Function to connect WebSocket |
| disconnect | () => void | Function to disconnect WebSocket |
| sendMessage | T(event: string, data: T) => boolean | Function to send a message |
| addEventListener | T(event: string, listener: (data: T) => void) => void | Add event listener |
| removeEventListener | T(event: string, listener: (data: T) => void) => void | Remove event listener |
| joinRoom | (roomName: string, participantId: string) => boolean | Join a room |
| requestToSpeak | (roomName: string, participantId: string, name: string, walletAddress: string) => boolean | Request to speak |
| inviteGuest | (roomName: string, participantId: string) => boolean | Invite guest to speak |
| returnToGuest | (roomName: string, participantId: string) => boolean | Return participant to guest role |
| actionExecuted | (roomName: string, actionId: string) => boolean | Mark action as executed |
| sendReaction | T(roomName: string, reaction: string, sender: T) => boolean | Send a reaction |
| startAddon | T(type: "Custom" \| "Q&A" \| "Poll" \| "Quiz", data?: T) => boolean | Start an addon |
| stopAddon | (type: "Custom" \| "Q&A" \| "Poll" \| "Quiz") => boolean | Stop an addon |

#### Description

The `useWebSocket` hook provides low-level WebSocket functionality for real-time communication. Most applications will use higher-level hooks that internally use this hook rather than using it directly. It handles:

- Connection establishment and reconnection
- Message parsing and dispatch
- Event-based message handling
- Automatic ping/pong handling
- Room-specific functionality