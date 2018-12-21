# Branch.io

Branch makes it simple to enable Universal Links all while greatly improving on them, offering full attribution, supporting edge cases where Universal Links fail (common) and allowing you to deep link when the user doesn't have your app installed.


# Configure your dashboard 

Basic integration :

- Go to Account settings on the Branch Dashboard. This branch key have to be mentioned in info.plist.
- Branch Keys allow you to interact with your Branch SDKs and create deep links
- These keys are unique to your Branch app
- Never expose your Branch Secret as it can be maliciously used.

# Configure your app

Configure Branch :
- Complete the Basic integration within Configure your dashboard
- Make sure I have an iOS app is enabled

- Configure Bundle Identifier : Make sure Bundle Id matches your Branch Dashboard

- Configure associated domains : Add your link domains from your Branch Dashboard

- Configure entitlements : Confirm entitlements are within target

- Configure Info.plist : Add Branch Dashboard values
    1. Add branch_app_domain with your live key domain
    2. Add branch_key with your current Branch key
    3. Add your URI scheme as URL Types -> Item 0 -> URL Schemes
    
- Confirm app prefix : From your Apple Developer Account

- Install Branch - Using CocoaPods

        platform :ios, '8.0'

        target 'APP_NAME' do
        # if swift
        use_frameworks!

        pod 'Branch'
        end

    pod install && pod update
    
    
- Initialize Branch

        import UIKit
        import Branch

        @UIApplicationMain
        class AppDelegate: UIResponder, UIApplicationDelegate {

        var window: UIWindow?

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        // if you are using the TEST key
        Branch.setUseTestBranchKey(true)
        // listener for Branch Deep Link data
        Branch.getInstance().initSession(launchOptions: launchOptions) { (params, error) in
        // do stuff with deep link data (nav to page, display content, etc)
        print(params as? [String: AnyObject] ?? {})
        }
        return true
        }

        func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
        Branch.getInstance().application(app, open: url, options: options)
        return true
        }

        func application(_ application: UIApplication, continue userActivity: NSUserActivity, restorationHandler: @escaping ([Any]?) -> Void) -> Bool {
        // handler for Universal Links
        Branch.getInstance().continue(userActivity)
        return true
        }

        func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
        // handler for Push Notifications
        Branch.getInstance().handlePushNotification(userInfo)
        }


- Test deep link

    1. Create a deep link from the Branch Dashboard
    2. Delete your app from the device
    3. Compile and test on a device
    4. Paste deep link in Apple Notes
    5. Long press on the deep link (not 3D Touch)
    6. Click Open in "APP_NAME" to open your app




