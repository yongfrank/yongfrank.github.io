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

Unlike explicitly requesting authorization, this code doesn’t prompt the user for permission to receive notifications. Instead, the first time you call this method, it automatically grants authorization. However, until the user either explicitly keeps or turns off the notification, the authorization status is UNAuthorizationStatus.provisional. Because users can change the authorization status at any point, you should still check the status before scheduling local notifications.

### Authorization Status

> [Customize notifications based on the current authorizations](https://developer.apple.com/documentation/usernotifications/asking_permission_to_use_notifications#2941986)

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

The above example uses a guard condition to prevent the scheduling of notifications if the app isn’t authorized. The code then configures the notification based on the types of interactions allowed, preferring the use of an alert-based notification whenever possible.

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

## Open System Settings

> * [iOS获取通知状态并跳转设置界面设置](https://www.jianshu.com/p/ddeebec23e90)
>
> * [How do I open phone settings when a button is clicked?](https://stackoverflow.com/questions/28152526/how-do-i-open-phone-settings-when-a-button-is-clicked)
>
> * [Opening app's notification settings in the settings app](https://stackoverflow.com/questions/42848539/opening-apps-notification-settings-in-the-settings-app)

## System Notification Authorization Status

```objc
#pragma mark - Notifications
/// 添加系统通知状态的观察者
- (void)addObserverForSystemNotificationStatus {
    [[NSNotificationCenter defaultCenter] addObserver:self
                                             selector:@selector(checkNotificationAuthorization)
                                                 name:UIApplicationWillEnterForegroundNotification
                                               object:nil];
}
// 在 checkNotificationAuthorization 方法中检查通知权限
- (void)checkNotificationAuthorization {
    [[UNUserNotificationCenter currentNotificationCenter] getNotificationSettingsWithCompletionHandler:^(UNNotificationSettings * _Nonnull settings) {
        dispatch_async(dispatch_get_main_queue(), ^{
            BOOL newAuthorizationStatus = self.isSystemNotificationOn;
            if (settings.authorizationStatus == UNAuthorizationStatusAuthorized) {
                newAuthorizationStatus = YES;
            } else {
                newAuthorizationStatus = NO;
            }
            // iOS 12.0 新增了 Provisional 的状态，无需获得用户授权也可发送 Deliver Quietly 的通知
            if (@available(iOS 12.0, *)) {
                if (settings.authorizationStatus == UNAuthorizationStatusProvisional) {
                    newAuthorizationStatus = YES;
                }
            }
            
            // 刷新表格
            if (newAuthorizationStatus != self.isSystemNotificationOn) {
                self.isSystemNotificationOn = newAuthorizationStatus;
                [self.tableView reloadData];
            }
        });
    }];
}
```

## Open App Settings From System Settings

> [providesAppNotificationSettings](https://developer.apple.com/documentation/usernotifications/unauthorizationoptions/2990405-providesappnotificationsettings)

```objc
- (void)userNotificationCenter:(UNUserNotificationCenter *)center openSettingsForNotification:(UNNotification *)notification {
    NSURL *url = [NSURL URLWithString:@"https://google.com"];
    [self openTrdPartyURL:url];
}
```

iOS 12.0+

An option indicating the system should display a button for in-app notification settings.

> [Preparing Your App For iOS 12 Notifications](https://www.smashingmagazine.com/2018/09/preparing-your-app-for-ios-12-notifications/)

![Deep link to to custom notification settings for NotificationTester from notification in the Notification Center.](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_80/w_2000/https://archive.smashing.media/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/c4ee20c1-ca99-419b-bfa4-4f5ebbffabf3/app-ios-12-notifications-turnoffnotifications.png)

![System Notification Settings](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_80/w_2000/https://archive.smashing.media/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/45e51038-8365-4064-ab03-feee8cc56eef/app-ios-12-notifications-systemnotificationsettings.png)
