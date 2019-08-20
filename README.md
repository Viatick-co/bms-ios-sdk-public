### XCode Application Frameworks
* UserNotifications.framework
* CoreLocation.framework
* NotificationCenter.framework

### Installation
* Download the `BmSDK.framework` from Github
* Link the framework `BmsSDK.framework` in your project. `Project target -> Under Embedded Binaries -> '+' Sign`
* Make sure `Info.plist` file has the following setting
```xml
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>description for location access</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>description for location access</string>
<key>NSLocationAlwaysUsageDescription</key>
<string>description for location access</string>
```
*  under `Capabilities` tag 
Enable `Location updates` in `Background Modes`
Enable `Add the Push Notifcations...` in `Push Notifications`

### Setup
##### Sample setup codes in ViewController
`swift 3`
```swift
class ViewController: UIViewController {

	// Some codes...
    
    let viaBmsCtrl = ViaBmsCtrl.sharedInstance;

    override func viewDidLoad() {
        super.viewDidLoad()

        // Some codes...

        // optional delegate
        viaBmsCtrl.delegate = self;

        // you can specify your customer information in order to enable attendance and tracking feature (optional)
        viaBmsCtrl.initCustomer(identifier: "qe7sua", phone: "9069xxxx", email: "customer@example.com");

        // bms sdk setting (setting can change later and default values are false)
        viaBmsCtrl.setting(alert: true, background: true, site: true, attendance: true, tracking: true);

        // initiate viatick bms sdk with your bms application sdk key (this function will not start the sdk service)
        viaBmsCtrl.initSdk(uiViewController: self, sdk_key: "PASTE_YOUR_BMS_APP_SDK_KEY_HERE");

        // Some codes...
    }

    // Some codes...

    // start sdk service
    @IBAction func startSDK(_ sender: Any) {
        viaBmsCtrl.startBmsService();
    }
    
    // end sdk service
    @IBAction func stopSDK(_ sender: Any) {
        viaBmsCtrl.stopBmsService();
    }

    // Some codes...

}

extension ViewController: ViaBmsCtrlDelegate {
    func viaBmsCtrl(controller: ViaBmsCtrl, inited status: Bool) {
        print("inited", status);
    }
}
```
