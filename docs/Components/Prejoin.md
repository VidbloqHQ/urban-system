### Prejoin

A component for the pre-join experience where users can set up their camera, microphone, and display name before joining a stream.

```jsx
import { Prejoin } from '@vidbloq/react';

function PreJoinScreen() {
  const handleJoin = () => {
    console.log('Joining stream...');
  };
  
  return (
    <Prejoin 
      onJoin={handleJoin}
      initialNickname="Guest User"
    />
  );
}
```

#### Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| onJoin | () => void | No | undefined | Callback when user joins the stream |
| initialNickname | string | No | "" | Initial value for nickname field |
| publicKey | PublicKey | No | null | Optional wallet public key |
| showWalletConnect | boolean | No | true | Whether to show wallet connect option |
| customPreview | React.ReactNode | No | undefined | Custom preview component |