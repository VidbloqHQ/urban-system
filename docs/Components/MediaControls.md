## Media Control Components

### MicrophoneControl

Toggle control for microphone.

```jsx
import { MicrophoneControl } from '@vidbloq/react';

function MyControls() {
  return (
    <MicrophoneControl 
      showLabel={true} 
      labelText="Microphone"
      onChange={(enabled) => console.log('Mic:', enabled)}
    />
  );
}
```

#### Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| className | string | No | "" | Additional CSS classes |
| style | React.CSSProperties | No | undefined | Inline styles |
| showLabel | boolean | No | true | Whether to show text label |
| labelText | string | No | "Mic" | Text to display as label |
| onChange | (enabled: boolean) => void | No | undefined | Callback when state changes |
| icon |  enabled?: React.ReactNode, disabled?: React.ReactNode  | No | undefined | Custom icons |

### CameraControl

Toggle control for camera.

```jsx
import { CameraControl } from '@vidbloq/react';

function MyControls() {
  return (
    <CameraControl 
      showLabel={true} 
      labelText="Camera"
      onChange={(enabled) => console.log('Camera:', enabled)}
    />
  );
}
```

#### Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| className | string | No | "" | Additional CSS classes |
| style | React.CSSProperties | No | undefined | Inline styles |
| showLabel | boolean | No | true | Whether to show text label |
| labelText | string | No | "Camera" | Text to display as label |
| onChange | (enabled: boolean) => void | No | undefined | Callback when state changes |
| icon |  enabled?: React.ReactNode, disabled?: React.ReactNode  | No | undefined | Custom icons |

### ScreenShareControl

Toggle control for screen sharing.

```jsx
import { ScreenShareControl } from '@vidbloq/react';

function MyControls() {
  return (
    <ScreenShareControl 
      showLabel={true} 
      labelText="Share Screen"
      onChange={(enabled) => console.log('Screen Share:', enabled)}
    />
  );
}
```

#### Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| className | string | No | "" | Additional CSS classes |
| style | React.CSSProperties | No | undefined | Inline styles |
| showLabel | boolean | No | true | Whether to show text label |
| labelText | string | No | "Screen" | Text to display as label |
| onChange | (enabled: boolean) => void | No | undefined | Callback when state changes |
| icon |  enabled?: React.ReactNode, disabled?: React.ReactNode  | No | undefined | Custom icons |

### RecordControl

Toggle control for recording.

```jsx
import { RecordControl } from '@vidbloq/react';

function MyControls() {
  const [isRecording, setIsRecording] = useState(false);
  
  return (
    <RecordControl 
      isRecording={isRecording}
      toggleRecording={() => setIsRecording(!isRecording)}
      showLabel={true} 
      labelText="Record"
    />
  );
}
```

#### Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| className | string | No | "" | Additional CSS classes |
| style | React.CSSProperties | No | undefined | Inline styles |
| showLabel | boolean | No | true | Whether to show text label |
| labelText | string | No | "Record" | Text to display as label |
| isRecording | boolean | Yes | - | Current recording state |
| toggleRecording | () => void | Yes | - | Function to toggle recording |

### MediaControls

A combined component that includes mic, camera, and screen sharing controls.

```jsx
import { MediaControls } from '@vidbloq/react';

function MyCallUI() {
  return (
    <MediaControls 
      showLabels={true}
      onChange={{
        mic: (enabled) => console.log('Mic:', enabled),
        camera: (enabled) => console.log('Camera:', enabled),
        screenShare: (enabled) => console.log('Screen share:', enabled),
      }}
    />
  );
}
```

#### Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| className | string | No | "" | Additional CSS classes |
| style | React.CSSProperties | No | undefined | Inline styles |
| showLabels | boolean | No | true | Whether to show text labels |
| onChange |  mic?: (enabled: boolean) => void, camera?: (enabled: boolean) => void, screenShare?: (enabled: boolean) => void  | No | undefined | Callbacks for changes |