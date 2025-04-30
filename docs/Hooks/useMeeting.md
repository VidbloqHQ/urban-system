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
  
  const hasAgenda = true; // Example
  const layoutType = calculateLayoutType(hasAgenda);
  const bottomRowParticipants = getBottomRowParticipants();
  
  // Use this data to build a custom meeting UI
  return (
    <div className={`meeting-layout ${layoutType}`}>
      {/* Main content */}
      <div className="main-content">
        {screenShareTrack ? (
          <ScreenShare track={screenShareTrack} />
        ) : (
          hostTrack && <ParticipantVideo track={hostTrack} />
        )}
        
        {hasAgenda && <AgendaPanel />}
      </div>
      
      {/* Bottom row participants */}
      <div className="participants-row">
        {bottomRowParticipants.map(track => (
          <ParticipantThumbnail 
            key={track.participant.identity}
            track={track}
            isActive={activeSpeaker === track.participant.identity}
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
| room | Room | LiveKit room instance |
| activeSpeaker | string \| null | Identity of current active speaker |
| rawTracks | TrackReference[] | All available tracks |
| cameraTracks | TrackReference[] | All camera tracks |
| screenShareTrack | TrackReference \| undefined | Active screen share track |
| hostTrack | TrackReference \| undefined | Host's track |
| coHostTracks | TrackReference[] | Co-hosts' tracks |
| displayedCoHosts | TrackReference[] | Co-hosts to display (limited) |
| overflowCount | number | Number of hidden co-hosts |
| getBottomRowParticipants | () => TrackReference[] | Function to get participants for bottom row |
| calculateLayoutType | (hasAgenda: boolean) => string | Function to determine optimal layout |
| overflowTracks | TrackReference[] | Tracks not shown in the main UI |

#### Description

The `useMeeting` hook provides optimized data for rendering a meeting interface. It:

- Separates host from co-hosts
- Handles screen sharing views
- Calculates optimal layout based on participant count and agenda
- Manages overflow for large meetings
- Provides functions for determining layout type and participant positioning

This hook is ideal for building custom meeting UIs with complex layouts.