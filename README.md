# ElkaAds-iOS

![Badge w/ Version](https://img.shields.io/cocoapods/v/ElkaAds.svg?style=flat)

## Step1 - Installing Cocoapods:

This project uses Cocoapods as its dependency manager. If you don’t have Cocoapods installed on your system, you can install it by this command in terminal:
```sh
$ sudo gem install cocoapods
```

After installation, you need to setup the cocoapods master repo. Type in terminal:
```sh
$ pod setup
```

And wait it will download the master repo after that:
```sh
$ cd "Your Xcode project root directory"
```

To update master repo:
```sh
$ pod repo update
```

Initialize Podfile:
```sh
$ pod init
```

Then open it using Xcode:
```sh
$ open -a Xcode Podfile
```

In Podfile uncomment  `platform :ios, '8.0'` and  `user_frameworks!` if you're using Swift

Add ElkaAds library:

`pod 'ElkaAds'`

Then in terminal:
```sh
$ pod install
```

> If framework version is not latest, run this in terminal: `pod update`


## Step2 - Updating Info.plist:

Add your given appID in info.plist:
```xml
<key>GADApplicationIdentifier</key>
<string>"Your_appID"</string>
```

Also add http Access:
```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

## Step3 - Initialize providers:

> For better result initialize providers as soon as possible like in `AppDelegate`

```swift
import ElkaAds

ElkaAdsProviders.InitializeProviders {
    print("provider initialized")
}
```

## Step4 - Initialize ads:

> Note: in debug mode make sure you checked test mode in user panel

### Video reward ad:

Initialize video reward ad:
```swift
private let rewardAd = ElkaAdsVideoRewardAd(elkaID: "Your_video_reward_ElkaID")
```

Then:
```swift

// Set ad delegate
rewardAd.delegate = self

// Verify ElkaID
rewardAd.verifyElkaID(onError: { (elkaError) in
    print(elkaError.errorCode)
    print(elkaError.errorMessage)
}) {
    print("elkaID Verified")
    
    // Load ad
    self.rewardAd.load()
}
```

> Every time you want to refresh ad you should call  `rewardAd.load()`

Implementing delegate:

```swift
extension SomeClassVC: ElkaAdsVideoRewardAdDelegate {
    func videoRewardAd(didRewardUserWith reward: ElkaAdsReward) {
        print("didRewardUser")
         
        guard let rewardType = reward.rewardType else {
            print("you should set reward type in your user panel")
            return
        }
        
        guard let rewardAmount = reward.rewardAmount else {
            print("you should set reward amount in your user panel")
            return
        }
        
        print(rewardType)
        print(rewardAmount)
    }
    
    func videoRewardAdDidOpen() {
        print("videoRewardAdDidOpen")
    }
    
    func videoRewardAdDidClose() {
        print("videoRewardAdDidClose")
    }
    
    func videoRewardAdDidReceive() {
        print("videoRewardAdDidReceive")
    }
    
    func videoRewardAdDidStartPlaying() {
        print("videoRewardAdDidStartPlaying")
    }
    
    func videoRewardAdDidCompletePlaying() {
        print("videoRewardAdDidCompletePlaying")
    }
    
    func videoRewardAdWillLeaveApplication() {
        print("videoRewardAdWillLeaveApplication")
    }
    
    func videoRewardAd(didFailedToLoadWithError elkaError: ElkaError) {
        print("ad did failed to load")
        print(elkaError.errorCode)
        print(elkaError.errorMessage)
    }
    
    func videoRewardAd(elkaDidFailedToLoadWithError elkaError: ElkaError) {
        print("Elka did failed to load")
        print(elkaError.errorCode)
        print(elkaError.errorMessage)
    }
}
```

> To check if ad is ready you can use `rewardAd.isReady`

To present ad:

```swift
if rewardAd.isReady {
    // Ad must present on your root viewController
    rewardAd.presentAd(rootViewController: self)
}
```

### Interstitial ad:

Initialize Interstitial ad:
```swift
private let interstitialAd = ElkaAdsInterstitialAd(elkaID: "Your interstitial ElkaID")
```

Then:
```swift

// Set ad delegate
interstitialAd.delegate = self

// Verify ElkaID
interstitialAd.verifyElkaID(onError: { (elkaError) in
    print(elkaError.errorCode)
    print(elkaError.errorMessage)
}) {
    print("elkaID Verified")
    
    // Load ad
    self.interstitialAd.load()
}
```

> Every time you want to refresh ad you must call  `interstitialAd.load()`

Implementing delegate:

```swift
extension SomeClassVC: ElkaAdsInterstitialAdDelegate {
    func interstitialAdWillLeaveApplication(reward: ElkaAdsReward) {
        print("AdWillLeaveApplication")
         
        guard let rewardType = reward.rewardType else {
            print("you should set reward type in your user panel")
            return
        }
        
        guard let rewardAmount = reward.rewardAmount else {
            print("you should set reward amount in your user panel")
            return
        }
        
        print(rewardType)
        print(rewardAmount)
    }
    
    func interstitialAdDidLoad() {
        print("interstitialAdDidLoad")
    }
    
    func interstitialAdDidClose() {
        print("interstitialAdDidClose")
    }
    
    func interstitialAdWillOpen() {
        print("interstitialAdWillOpen")
    }
    
    func interstitialAd(didFailedToLoadWithError elkaError: ElkaError) {
        print("ad did failed to load")
        print(elkaError.errorCode)
        print(elkaError.errorMessage)
    }
    
    func interstitialAd(elkaDidFailedToLoadWithError elkaError: ElkaError) {
        print("Elka did failed to load")
        print(elkaError.errorCode)
        print(elkaError.errorMessage)
    }
}
```

> To check if ad is ready you can use `interstitialAd.isReady`

To present ad:

```swift
if interstitialAd.isReady {
    // Ad must presented on your root viewController
    interstitialAd.presentAd(rootViewController: self)
}
```
