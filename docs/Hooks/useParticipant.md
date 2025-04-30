### useParticipantList

Provides a list of participants in the current stream with status information.

```jsx
import { useParticipantList } from '@vidbloq/react';

function ParticipantListComponent() {
  const { 
    participants, 
    localParticipant, 
    hostParticipant, 
    coHostParticipants, 
    guestParticipants,
    isLoading,
    error
  } = useParticipantList();
  
  if (isLoading) return <div>Loading participants...</div>;
  if (error) return <div>Error loading participants: {error.message}</div>;
  
  return (
    <div className="participants-panel">
      <h3>Participants ({participants.length})</h3>
      
      <div className="host-section">
        <h4>Host</h4>
        {hostParticipant && (
          <ParticipantItem 
            participant={hostParticipant}
            isLocal={hostParticipant.identity === localParticipant?.identity}
          />
        )}
      </div>
      
      <div className="co-hosts-section">
        <h4>Co-Hosts ({coHostParticipants.length})</h4>
        {coHostParticipants.map(participant => (
          <ParticipantItem 
            key={participant.identity}
            participant={participant}
            isLocal={participant.identity === localParticipant?.identity}
          />
        ))}
      </div>
      
      <div className="guests-section">
        <h4>Guests ({guestParticipants.length})</h4>
        {guestParticipants.map(participant => (
          <ParticipantItem 
            key={participant.identity}
            participant={participant}
            isLocal={participant.identity === localParticipant?.identity}
          />
        ))}
      </div>
    </div>
  );
}
```

#### Return Value

| Property | Type | Description |
|----------|------|-------------|
| participants | ParticipantInfo[] | All participants in the stream |
| localParticipant | ParticipantInfo \| null | The current user's participant info |
| hostParticipant | ParticipantInfo \| null | The host participant |
| coHostParticipants | ParticipantInfo[] | List of co-host participants |
| guestParticipants | ParticipantInfo[] | List of guest participants |
| isLoading | boolean | Whether participant data is loading |
| error | Error \| null | Any error that occurred |

#### Description

The `useParticipantList` hook provides access to the list of participants in the current stream, grouped by their roles. It:

- Tracks all participants in real-time
- Categorizes participants by role (host, co-host, guest)
- Identifies the local participant
- Provides loading and error states

This hook is ideal for building participant lists, control panels, and user management interfaces.