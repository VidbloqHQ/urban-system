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
  return (
    <div className="livestream-container">
      {/* Main content area */}
      <div className="main-video">
        {mainContent && <ParticipantVideo track={mainContent} />}
      </div>
      
      {/* Sidebar with other participants */}
      <div className="sidebar">
        {sidebarContent.map(track => (
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
| rawTracks | TrackReference[] | All available tracks |
| activeSpeaker | string \| null | Identity of current active speaker |
| screenShareTrack | TrackReference \| undefined | Active screen share track |
| screenSharerIdentity | string \| undefined | Identity of screen sharer |
| participantsByType | Record string, ParticipantTrack[] | Participants grouped by type |
| mainContent | ParticipantTrack \| null | Track to display in main view |
| sidebarContent | ParticipantTrack[] | Tracks to display in sidebar |
| room | Room | LiveKit room instance |

#### Description

The `useLivestream` hook provides optimized data for rendering a livestream interface. It:

- Tracks participant cameras and screen shares
- Groups participants by role (host, co-host, guest)
- Determines optimal layout (main content vs sidebar)
- Detects active speakers
- Provides access to the underlying LiveKit room

This hook is ideal for building custom livestream UIs with optimal participant positioning.