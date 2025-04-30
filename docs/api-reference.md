# API Reference

This section provides detailed technical documentation for the Vidbloq SDK API, including types, classes, and utility functions.

## Core Types

### Tenant Types

```typescript
interface Tenant {
  id: string;
  name: string;
  email: string;
  createdAt: string;
  updatedAt: string;
  status: "active" | "suspended" | "pending";
  plan: "free" | "starter" | "pro" | "enterprise";
  features: string[];
  branding?: {
    logo?: string;
    colors?: {
      primary?: string;
      secondary?: string;
    };
  };
}

interface TenantResponse {
  tenant: Tenant;
  success: boolean;
}

interface TenantContextType {
  apiKey: string;
  apiSecret: string;
  websocket: ReturnType<typeof useWebSocket>;
  isConnected: boolean;
  connect: () => Promise<void>;
  disconnect: () => void;
  apiClient: ApiClient;
  tenant: Tenant | null;
  isLoading: boolean;
}
```

### Stream Types

```typescript
interface StreamMetadata {
  title: string;
  callType: string;
  creatorWallet: string;
  streamSessionType: string;
}

interface GuestRequest {
  participantId: string;
  name: string;
  walletAddress: string;
  timestamp: number;
}

interface Agenda {
  id: string;
  title: string;
  description?: string;
  timestamp: number;
  duration?: number;
  completed: boolean;
}

interface TokenResponse {
  token: string;
  userType: UserType;
}

interface GetStreamResponse {
  title: string;
  callType: string;
  streamSessionType: string;
  creatorWallet: string;
}

type UserType = "host" | "co-host" | "guest" | "temp-host" | null;

interface ParticipantMetadata {
  userType?: UserType;
  name?: string;
  walletAddress?: string;
}

interface ParticipantTrack {
  participant: {
    identity: string;
    metadata: string;
  };
  publication: {
    isEnabled: boolean;
    isSubscribed: boolean;
  };
  source: string;
}

type CallType = "video" | "audio";
type StreamSessionType = "livestream" | "meeting";
type StreamFundingType = "free" | "paid" | "subscription";

interface CreateStreamResponse {
  id: string;
  roomName: string;
  createdAt: string;
  status: "active" | "scheduled" | "ended";
  title: string;
  callType: CallType;
  streamSessionType: StreamSessionType;
  fundingType: StreamFundingType;
  isPublic: boolean;
  scheduledFor?: string;
  hostToken?: string;
}
```

### Wallet Types

```typescript
interface WalletSigner {
  signTransaction: (transaction: Transaction) => Promise<Transaction>;
}

interface WalletContextType {
  publicKey: PublicKey | null;
  connectWallet: (key: PublicKey, signer?: WalletSigner) => void;
  clearWallet: () => void;
  signTransaction?: (transaction: Transaction) => Promise<Transaction>;
  connected: boolean;
}

interface Recipient {
  publicKey: PublicKey;
  amount: number;
}
```

## API Client

The SDK provides an `ApiClient` class for making authenticated API requests:

```typescript
class ApiClient {
  constructor(apiKey: string, apiSecret: string, baseUrl: string);
  
  async get<T>(endpoint: string, params?: Record<string, any>): Promise<T>;
  async post<T>(endpoint: string, data: any): Promise<T>;
  async put<T>(endpoint: string, data: any): Promise<T>;
  async delete<T>(endpoint: string): Promise<T>;
}
```

### Usage Example

```javascript
const { apiClient } = useTenantContext();

// GET request
const streams = await apiClient.get('/streams');

// POST request
const newStream = await apiClient.post('/stream', {
  title: 'My New Stream',
  callType: 'video',
  streamSessionType: 'livestream'
});

// PUT request
await apiClient.put(`/stream/${streamId}`, {
  title: 'Updated Stream Title'
});

// DELETE request
await apiClient.delete(`/stream/${streamId}`);
```

## WebSocket Client

The SDK uses a WebSocket client for real-time communication:

### WebSocket API

```typescript
interface WebSocketClient {
  isConnected: boolean;
  connect(): void;
  disconnect(): void;
  sendMessage<T>(event: string, data: T): boolean;
  addEventListener<T>(event: string, listener: (data: T) => void): void;
  removeEventListener<T>(event: string, listener: (data: T) => void): void;
  joinRoom(roomName: string, participantId: string): boolean;
  requestToSpeak(roomName: string, participantId: string, name: string, walletAddress: string): boolean;
  inviteGuest(roomName: string, participantId: string): boolean;
  returnToGuest(roomName: string, participantId: string): boolean;
  actionExecuted(roomName: string, actionId: string): boolean;
  sendReaction<T>(roomName: string, reaction: string, sender: T): boolean;
  startAddon<T>(type: "Custom" | "Q&A" | "Poll" | "Quiz", data?: T): boolean;
  stopAddon(type: "Custom" | "Q&A" | "Poll" | "Quiz"): boolean;
}
```

### WebSocket Events

The WebSocket client handles the following events:

| Event | Description | Data |
|-------|-------------|------|
| `auth` | Authentication message | `{ apiKey: string, apiSecret: string }` |
| `authResponse` | Authentication response | `{ success: boolean, error?: string }` |
| `joinRoom` | Join a room | `{ roomName: string, participantId: string }` |
| `leaveRoom` | Leave a room | `{ roomName: string, participantId: string }` |
| `requestToSpeak` | Request to speak | `{ roomName: string, participantId: string, name: string, walletAddress: string }` |
| `inviteGuest` | Invite guest to speak | `{ roomName: string, participantId: string }` |
| `returnToGuest` | Return participant to guest role | `{ roomName: string, participantId: string }` |
| `guestRequestsUpdate` | Guest requests update | `GuestRequest[]` |
| `timeSync` | Time synchronization | `number` (current time in seconds) |
| `initialSync` | Initial state sync | `{ currentTime: number, executedActions: string[], joinTime: number }` |
| `newToken` | New token issued | `{ token: string }` |
| `ping` | Server ping | `{}` |
| `pong` | Client pong response | `{}` |

### Usage Example

```javascript
const { websocket } = useTenantContext();

// Add event listener
websocket.addEventListener('customEvent', (data) => {
  console.log('Custom event received:', data);
});

// Send a message
websocket.sendMessage('customAction', { 
  roomName: 'my-room',
  actionType: 'highlight',
  targetId: 'user-123'
});

// Join a room
websocket.joinRoom('my-room', 'user-123');

// Request to speak
websocket.requestToSpeak(
  'my-room',
  'user-123',
  'John Doe',
  'wallet-address-123'
);
```

## Wallet Utilities

### Wallet Registration

The SDK provides utilities for registering and managing wallet adapters:

```typescript
function registerWalletAdapter(name: string, adapter: WalletSigner): void;
function getWalletAdapter(name: string): WalletSigner | undefined;
function isWalletRegistered(name: string): boolean;
```

### Usage Example

```javascript
import { registerWalletAdapter } from '@vidbloq/sdk';
import { PhantomWalletAdapter } from '@solana/wallet-adapter-phantom';

// Register a custom wallet adapter
const phantomAdapter = new PhantomWalletAdapter();
await phantomAdapter.connect();

registerWalletAdapter('phantom', {
  signTransaction: phantomAdapter.signTransaction.bind(phantomAdapter)
});

// Later, use the registered adapter
const { connectWallet } = useWalletContext();
connectWallet(publicKey, getWalletAdapter('phantom'));
```

## Constants and Configuration

### Base API URL

```javascript
export const baseApi = 'https://api.vidbloq.com/v1';
```

### WebSocket URL

```javascript
export const websocketUrl = 'wss://ws.vidbloq.com';
```

## Error Handling

The SDK uses standard Error objects for error handling:

```typescript
// Example of error handling in hooks
function useExample() {
  const [error, setError] = useState<Error | null>(null);
  
  const doSomething = async () => {
    try {
      // Operation that might fail
    } catch (err) {
      // Convert to Error object if needed
      const error = err instanceof Error ? err : new Error(String(err));
      setError(error);
      throw error; // Re-throw for caller handling
    }
  };
  
  return { doSomething, error };
}
```

## LiveKit Integration

The SDK integrates with LiveKit for video streaming:

### LiveKit Room Connection

```typescript
<LiveKitRoom
  audio={boolean}        // Whether to enable audio
  video={boolean}        // Whether to enable video
  token={string}         // Authentication token
  serverUrl={string}     // LiveKit server URL
  // Other LiveKit options
/>
```

### Track Sources

```typescript
enum TrackSource {
  Camera = 'camera',
  Microphone = 'microphone',
  ScreenShare = 'screen_share',
  ScreenAudio = 'screen_share_audio'
}
```

### Track Reference

```typescript
interface TrackReference {
  participant: {
    identity: string;
    metadata: string;
  };
  publication: {
    isEnabled: boolean;
    isSubscribed: boolean;
    track: Track;
  };
  source: TrackSource;
}
```

## Environment Variables

The SDK supports the following environment variables:

| Variable | Description | Default |
|----------|-------------|---------|
| `VIDBLOQ_API_URL` | Base API URL | https://api.vidbloq.com/v1 |
| `VIDBLOQ_WS_URL` | WebSocket URL | wss://ws.vidbloq.com |
| `VIDBLOQ_LIVEKIT_URL` | LiveKit server URL | wss://streamlink-vtdavgse.livekit.cloud |

## Type Declarations

### Full Type Declarations

For complete type information, you can import types directly:

```typescript
import {
  Tenant,
  TenantResponse,
  TenantContextType,
  StreamMetadata,
  GuestRequest,
  Agenda,
  TokenResponse,
  GetStreamResponse,
  UserType,
  ParticipantMetadata,
  ParticipantTrack,
  CallType,
  StreamSessionType,
  StreamFundingType,
  CreateStreamResponse,
  WalletSigner,
  WalletContextType,
  Recipient
} from '@vidbloq/sdk/types';
```

## Advanced API Features

### Custom WebSocket URL

You can customize the WebSocket URL when initializing the SDK:

```jsx
<VidbloqProvider 
  apiKey="YOUR_API_KEY" 
  apiSecret="YOUR_API_SECRET"
  websocketUrl="wss://custom-websocket.example.com"
>
  {/* Your application */}
</VidbloqProvider>
```

### Custom Signer Registration

Register a custom transaction signer:

```javascript
import { registerWalletAdapter } from '@vidbloq/sdk';
import { PublicKey, Transaction } from '@solana/web3.js';

// Custom signer implementation
const customSigner = {
  signTransaction: async (transaction) => {
    // Custom signing logic
    return signedTransaction;
  }
};

// Register the custom signer
registerWalletAdapter('my-custom-wallet', customSigner);

// Use the custom signer
const { connectWallet } = useWalletContext();
connectWallet(new PublicKey('your-public-key'), customSigner);
```

## API Endpoints

The SDK interacts with the following API endpoints:

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/tenant/me/info` | GET | Get current tenant information |
| `/stream` | POST | Create a new stream |
| `/stream/:roomName` | GET | Get stream information |
| `/stream/token` | POST | Generate a stream token |
| `/pay` | POST | Create a payment transaction |
| `/pay/submit` | POST | Submit a signed transaction |

### Example API Responses

#### Get Tenant Information

```json
{
  "tenant": {
    "id": "tenant-123",
    "name": "Example Organization",
    "email": "admin@example.com",
    "createdAt": "2023-01-01T00:00:00Z",
    "updatedAt": "2023-05-15T12:34:56Z",
    "status": "active",
    "plan": "pro",
    "features": ["recording", "transcription", "customization"]
  },
  "success": true
}
```

#### Create Stream

```json
{
  "id": "stream-123",
  "roomName": "my-awesome-stream",
  "createdAt": "2023-05-20T14:30:00Z",
  "status": "active",
  "title": "Product Demo",
  "callType": "video",
  "streamSessionType": "livestream",
  "fundingType": "free",
  "isPublic": true,
  "hostToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

#### Generate Token

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "userType": "host"
}
```

## Error Codes

The API may return the following error codes:

| Code | Description |
|------|-------------|
| 400 | Bad Request - Invalid parameters |
| 401 | Unauthorized - Invalid API credentials |
| 403 | Forbidden - Insufficient permissions |
| 404 | Not Found - Resource not found |
| 409 | Conflict - Resource already exists |
| 429 | Too Many Requests - Rate limit exceeded |
| 500 | Internal Server Error |
| 503 | Service Unavailable - Temporary outage |

### Error Response Format

```json
{
  "error": {
    "code": "INVALID_PARAMETERS",
    "message": "Invalid stream parameters",
    "details": {
      "fields": ["title", "callType"]
    }
  },
  "success": false
}
```