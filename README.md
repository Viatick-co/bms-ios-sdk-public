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
##### Under `Capabilities` tag
* Enable `Location updates` in `Background Modes`
* Enable `Add the Push Notifcations...` in `Push Notifications`

### Setup

##### Import SDK in ViewController
`Import BmsSDK`

##### Sample setup codes in ViewController

`swift 3`
```swift
class ViewController: UIViewController {

    // Some codes...
    
    // declare instance of bms controller
    let viaBmsCtrl = ViaBmsCtrl.sharedInstance;

    override func viewDidLoad() {
        super.viewDidLoad()

        // Some codes...

	var requestDistanceBeacons:[IBeacon] = [];
        let beacon:IBeacon = IBeacon(uuid: "uuid", major: 40, minor: 50);

        // configure bms sdk settings at first
        // to enable alert
        // to enable minisite feature and type of view (AUTO or LIST)
        // to enable customer tracking feature
        // to enable customer attendance feature
	// to enable distance tracking then pass list of IBeacon, otherwise just pass null
        viaBmsCtrl.setting(alert: true, background: true, site: true, minisitesView: .LIST, autoSiteDuration: 0, tracking: true, enableMQTT: false, attendance: true, checkinDuration: 2, checkoutDuration: 2, requestDistanceBeacons: requestDistanceBeacons, bmsEnvironment: .DEV);
	
	// optional to attach delegate
        // 4 callbacks
        // sdkInited
        // customerInited
        // if attendance is enable
        // checkin and checkout
        viaBmsCtrl.delegate = self;
	
        // this method must be called at first to do handshake with bms
        // sdkInited callback will be called
        viaBmsCtrl.initSdk(uiViewController: self, sdk_key: "PASTE_YOUR_BMS_APP_SDK_KEY_HERE");

        // Some codes...
    }

    // start sdk service
    @IBAction func startSDK(sender: UIButton) {
        // these methods are to check sdk initation and bms is running or not
        let bmsRunning = viaBmsCtrl.isBmsRunning();
        let sdkInited = viaBmsCtrl.isSdkInited();

        if (!bmsRunning && sdkInited) {
            // this method is to start bms service if it is not running
            // you can call this method to restart without calling initSdk again
            viaBmsCtrl.startBmsService();
        }
    }
    
    // end sdk service
    @IBAction func stopSDK(sender: UIButton) {
        // this method is to stop the bms service
        viaBmsCtrl.stopBmsService();
    }

    // Some codes...
}

// implement delegate of bms here
extension ViewController: ViaBmsCtrlDelegate {
    
    // this will be called when sdk is inited
    // list of zones in the sdk application is passed here
    func sdkInited(inited status: Bool, zones: [ViaZone]) {
        print("sdk inited", status);
        
        // this method must be called in order to enable attendance and tracking feature
        // authorizedZones is optional field
	// sdkInited callback will be called after initialization
        viaBmsCtrl.initCustomer(identifier: "PASTE IDENTIFIER OF CUSTOMER HERE", email: "example@email.com", phone: "+000000000", remark: "Device info!", authorizedZones: zones);
    }
    
    func customerInited(inited: Bool) {
        print("customer inited", inited);
    }
    
    func checkin() {
        print("check in callback");
    }
    
    func checkout() {
        print("check out callback");
    }
    
    // it is callback of request tracking beacons
    func onDistanceBeacons(beacons: [IBeacon]) {
    }
}
```
