
# BondBotSDK Integration

## Using CocoaPods

BondBotSDKNew is available through CocoaPods. To install, add the following line to your Pod file and then use command pod install:

```ruby
pod ‘BondBotSDKNew’, ‘~> 1.3.1’
```
This pulls from the `master` branch directly.

Second, install `BondBotSDKNew` into your project:

```ruby
pod install
```

## How to use ?
1) Import BondBotAO.framework.

```swift
import BondBotAO
```

2.1) Call below method to open the Account opening ChatBot before login.

```swift
func navigateToBondBotControllerAO(){
        DispatchQueue.main.async {
            let storyBoard : UIStoryboard = UIStoryboard(name: "Chat", bundle:Bundle(for: BondBotAO.self))
            let bondbotController = storyBoard.instantiateInitialViewController() as! BondBotAO
            bondbotController.modalPresentationStyle = .fullScreen
            bondbotController.RequestedBot = "CONVERSATIONBOT"
            NotificationCenter.default.removeObserver(self)
            bondbotController.serverURL = self.serverURL
           //Set language code EN(for english) or ES(for spanish)
            bondbotController.langCode = "EN"
            self.present(bondbotController, animated: true, completion: nil)
        }
    }
```

2.2) Call below method to open the FAQ ChatBot. (After user logged in, If user wants to navigate to Open FAQ then send requestedBot parameter value as “FAQBOT”)

```swift
func navigateToBondBotController(){
          
        DispatchQueue.main.async {
            // Use these 3 lines as it is.
            let storyBoard = UIStoryboard (name: "Chat", bundle: Bundle(for: BondBotAO.self))
            let bondbotController = storyBoard.instantiateInitialViewController() as! BondBotAO
            bondbotController.modalPresentationStyle = .fullScreen
            
            //Set Server URL
            bondbotController.serverURL = self.serverURL!

            bondbotController.RequestedBot = "FAQBOT"
           
            //Set Username
            if self.username != nil {
                 bondbotController.userName = self.username!
            }
            //Set Access Token
            
            bondbotController.userAccessToken = self.accessToken ?? ""
            
            //Set language code EN(for english) or ES(for spanish)
            bondbotController.langCode = self.langCode
            
            //Set Email Address
            bondbotController.userEmail = self.userEmail ?? ""
            
            //Present ChatBot Controller
            self.present(bondbotController, animated: true, completion: nil)
        }

```



#Parameter values to be set by the parent application(all are mandatory except for External Account opening chatbot before login )

- userEmail -> User Email
- serverURL -> server URL  
- userName -> User Name
- authToken -> Authentication Token
- userAccessToken -> Access Token
- langCode -> Language Code
- RequestedBot - > requestedBot type (ie. “CONVERSATIONBOT” or “FAQBOT”)

Note: you can get above Values(ie:authtoken,userAccesstoken) by calling login Api which is demonstated in demo app.


2.3) Set colours & icons from parent app to the botSDK using the below method 
(If colors are not set from the Parent App, then default Bond Bot colors & icons will be applied)

```swift
func customiseBotUI() {
        //If color then set in HEX. i.e. “#0000FF" or “0000FF" //If icon then set i.e. "abc.png"
      	 botSdkConfigAO.VERSION_TEXT = ""
        botSdkConfigAO.VERSION_MESSAGE_TEXT_COLOR = "#6001D2"
        // set hide/show for show version.
        botSdkConfigAO.SHOWVERSION = true
        // view User msg background
        botSdkConfigAO.USER_MESSGAE_BACKGROUND_COLOR =  "e8e8e8"
	// set color for background view of bot message
        botSdkConfigAO.BOT_MESSGAE_BACKGROUND_COLOR =  "#EEEEEE"
        // set text color for user message
        botSdkConfigAO.USER_MESSGAE_TEXT_COLOR = "EEEEEE"
        // set text color for bot message
        botSdkConfigAO.BOT_MESSGAE_TEXT_COLOR = "4B4B4B"
        //set hyperlink color for bot message
        botSdkConfigAO.BOT_MESSAGE_URL_TEXT_COLOR = "6001D2"
        //set color for chat screen background. main view, header, footer.
        botSdkConfigAO.CHAT_SCREEN_BACKGROUND = "FFFFFF"
        // chatTextfield placeholder text
        botSdkConfigAO.INPUT_MSG_TEXT = "Say Something.."
        //chatTextfield placeholder color
        botSdkConfigAO.PLACEHOLDER_COLOR = "#A6A6A6"
        //chatTextfield background
        botSdkConfigAO.INPUT_FIELD_BACKGROUND = "#FFFFFF"
        //chatTextfield textcolor
        botSdkConfigAO.INPUT_FIELD_TEXT_COLOR = "#434343"
        // set icon for send button
        botSdkConfigAO.BTN_SEND_IMG =  "send.png"
        // Icon for bot message
        botSdkConfigAO.IMG_BOND_ICON =  "bot_Icon.png"
        // set background color for List message & related question.
        botSdkConfigAO.RELATED_MESSAGE_BACKGROUND = "FFFFFF"
        //set border color & text color for list message as well as related question. Text color for greet message.
        botSdkConfigAO.RELATED_MESSAGE_COLOR = "#009dac"
        // set icon for close button.
        botSdkConfigAO.BACK_IMAGE = “x_white.png"
        botSdkConfigAO.IMG_BOND_ICON_CAMERA = “ios-camera.png"
        botSdkConfigAO.Color_gradient_Top = "3985FF"
        botSdkConfigAO.Color_gradient_Bottom = "6001D2"
       }
```

2.4) Put following Keys in Info.plist file  of your app

```XML
	<key>ITSAppUsesNonExemptEncryption</key>
	<false/>
	<key>NSAppTransportSecurity</key>
	<dict>
		<key>NSAllowsArbitraryLoads</key>
		<true/>
	</dict>
	<key>NSCameraUsageDescription</key>
	<string>This app requires access to the camera.</string>
	<key>NSMicrophoneUsageDescription</key>
	<string>Require microphone access </string>
	<key>NSSpeechRecognitionUsageDescription</key>
	<string>Require speech recognition access </string>
```

2.5) before submitting on app store you need to insert following Run script given below, in build phases

```ruby
echo "\n ⏱ Removing Unused Architectures \n\n\n"

exec > /tmp/${PROJECT_NAME}_archive.log 2>&1


FRAMEWORK="BondBotAO"

FRAMEWORK_EXECUTABLE_PATH="${BUILT_PRODUCTS_DIR}/${FRAMEWORKS_FOLDER_PATH}/$FRAMEWORK.framework/$FRAMEWORK"

EXTRACTED_ARCHS=()

for ARCH in $ARCHS

do

lipo -extract "$ARCH" "$FRAMEWORK_EXECUTABLE_PATH" -o "$FRAMEWORK_EXECUTABLE_PATH-$ARCH"

EXTRACTED_ARCHS+=("$FRAMEWORK_EXECUTABLE_PATH-$ARCH")

done

lipo -o "$FRAMEWORK_EXECUTABLE_PATH-merged" -create "${EXTRACTED_ARCHS[@]}"

rm "${EXTRACTED_ARCHS[@]}"
rm "$FRAMEWORK_EXECUTABLE_PATH"
mv "$FRAMEWORK_EXECUTABLE_PATH-merged" "$FRAMEWORK_EXECUTABLE_PATH"

echo "\n ⏱ Removing Unused Architectures \n\n\n"
echo "\n\n\n 🏁 Completed removing unused architectures from your fat framework."
echo "\n\n\n 🔍 For more details please check the /tmp/${PROJECT_NAME}_archive.log file. \n\n\n"

```



## SDK Installation Requirements

- xcode 12
- Swift 5.0 or above
- iOS V11 or above



## Supported Devices (Following device with IOS 11 or above)

- IPhone 12,12 pro,12 pro Max
- IPhone 11,11 pro,11 pro Max
- IPhone XS,XS Max
- IPhone XR,X
- IPhone 8,8 Plus
- IPhone 7,7 Plus
- IPhone 6S,6S Plus,6 Plus
- IPhone SE
- IPhone 5S


## Server URL
- serverURL will be provided by Bond team.

For more Information Contact Mahesh@bond.ai

