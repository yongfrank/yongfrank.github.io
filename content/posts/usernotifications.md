---
title: "UserNotifications"
date: 2023-05-12T11:36:13+08:00
---

## Quick Start

### Scheduling local notifications

> [Scheduling local notifications](https://www.hackingwithswift.com/books/ios-swiftui/scheduling-local-notifications)

```txt
"NotificationsTesting" Would Like to Send You Notifications

Notifications may include alerts, sounds, and icon badges. These can be configured in Settings.

 --------------------------
| Dont't Allow |   Allow   |
 --------------------------
```

> [Asking permission to use notifications](https://developer.apple.com/documentation/usernotifications/asking_permission_to_use_notifications)

![Notifications](https://docs-assets.developer.apple.com/published/c8879aa786/3559454@2x.png)

The ability to post noninterrupting notifications provisionally to the Notification Center.

![Provisional Notifications](https://docs-assets.developer.apple.com/published/75a57b5a99/3544497@2x.png)

```swift
let center = UNUserNotificationCenter.current()
center.requestAuthorization(options: [.alert, .sound, .badge, .provisional]) { granted, error in
    
    if let error = error {
        // Handle the error here.
    }
    
    // Provisional authorization granted.
}
```

### Authorization Status

Unlike explicitly requesting authorization, this code doesn’t prompt the user for permission to receive notifications. Instead, the first time you call this method, it automatically grants authorization. However, until the user either explicitly keeps or turns off the notification, the authorization status is UNAuthorizationStatus.provisional. Because users can change the authorization status at any point, you should still check the status before scheduling local notifications.

```swift
let center = UNUserNotificationCenter.current()
center.getNotificationSettings { settings in
    guard (settings.authorizationStatus == .authorized) ||
          (settings.authorizationStatus == .provisional) else { return }

    if settings.alertSetting == .enabled {
        // Schedule an alert-only notification.
    } else {
        // Schedule a notification with a badge and sound.
    }
}
```

### Full Code

```swift
import SwiftUI
import UserNotifications
import AlertToast

struct ContentView: View {
    @State private var showToast = false
    @State private var alertTitle = ""
    var body: some View {
        VStack {
            Image(systemName: "globe")
                .imageScale(.large)
                .foregroundColor(.accentColor)
            Text("Hello, world!")
            Button("Request Permission") {
                // First, Authorization 
                UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .badge, .sound]) { success, error in
                    if success {
                        showToast.toggle()
                        alertTitle = "Success"
                    } else if let error = error {
                        print(error.localizedDescription)
                    }
                }
            }
            .alert(alertTitle, isPresented: $showToast) {}
            
            Button("Schedule Notification") {
                // Second, Schedule Notification
                let content = UNMutableNotificationContent()
                content.title = "Feed the cat"
                content.subtitle = "It looks hungry"
                content.sound = UNNotificationSound.default
                
                // show this notification five seconds from now
                let trigger = UNTimeIntervalNotificationTrigger(timeInterval: 5, repeats: false)
                
                // choose a random identifier
                let request = UNNotificationRequest(identifier: UUID().uuidString, content: content, trigger: trigger)
                
                // add our notification request
                UNUserNotificationCenter.current().add(request)
                
                showToast.toggle()
                alertTitle = "Notification is coming"
            }
        }
        .padding()
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```