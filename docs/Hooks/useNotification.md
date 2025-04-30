### useNotification

Provides a notification system for displaying messages to users.

```jsx
import { useNotification } from '@vidbloq/react';

function NotificationDemo() {
  const { addNotification, removeNotification, notifications } = useNotification();
  
  const showSuccessNotification = () => {
    addNotification({
      type: "success",
      message: "Operation completed successfully!",
      duration: 5000, // 5 seconds
    });
  };
  
  const showErrorNotification = () => {
    const id = addNotification({
      type: "error",
      message: "Something went wrong",
      duration: 0, // Won't auto-dismiss
    });
    
    // You can manually dismiss it later
    setTimeout(() => removeNotification(id), 10000);
  };
  
  return (
    <div>
      <button onClick={showSuccessNotification}>Show Success</button>
      <button onClick={showErrorNotification}>Show Error</button>
      
      {/* Display current notifications */}
      <div className="current-notifications">
        {notifications.map(notification => (
          <div key={notification.id} className={`notification ${notification.type}`}>
            {notification.message}
          </div>
        ))}
      </div>
    </div>
  );
}
```

#### Return Value

| Property | Type | Description |
|----------|------|-------------|
| addNotification | (data: NotificationData) => string | Function to add a notification (returns ID) |
| removeNotification | (id: string) => void | Function to remove a notification |
| notifications | Notification[] | List of current notifications |

#### NotificationData

| Property | Type | Required | Default | Description |
|----------|------|----------|---------|-------------|
| type | "success" \| "error" \| "info" \| "warning" | Yes | - | Type of notification |
| message | string | Yes | - | Notification message |
| duration | number | No | 3000 | Duration in ms (0 = no auto-dismiss) |

#### Description

The `useNotification` hook provides a system for displaying notifications to users. It:

- Manages notification creation and removal
- Handles auto-dismissing notifications
- Tracks all current notifications

This hook is used throughout the SDK for user feedback and can be used in your application for consistent notifications.