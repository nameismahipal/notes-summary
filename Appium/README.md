http://appium.io/

# Appium
**Appium** is 
  - an open source

  - cross-platform test automation tool for 
    - native apps
    - hybrid apps
    - mobile web apps

    - tested on 
      - simulators (iOS, FirefoxOS) 
      - emulators (Android)
      - real devices (iOS, Android, FirefoxOS).

Supported Platforms: Android, iOS, Firefox OS.

- Uses Standard Automation APIs (Cross-Platform), so there is no need to re-compile.
- Flexibility in using any Test Framework.
- Can be written in any Webdriver Supported Languages 
  - Java
  - C#
  - JavaScript
  - Python
  - Ruby

What is **WebDriver** Protocol ?
```
All implementations of WebDriver that communicate with the browser, or a RemoteWebDriver server shall use a common wire protocol. 

The JSON wire protocol (JSONWP) is a transport mechanism created by WebDriver developers.

This wire protocol is a specific set of predefined, standardized endpoints exposed via a RESTful API.

This wire protocol defines a RESTful web service using JSON over HTTP.

The wire protocol is implemented in request/response pairs of "commands" and "responses".

Appium implements the Mobile JSONWP, the extension to the Selenium JSONWP and it controls the different mobile device behaviors, such as installing/uninstalling apps over the session.
```
Advantage: Cross Platform Native Mobile Automation.

Apple's UIAutomation Library 
  - can write tests only in JS
  - run tests only through Instruments Application.

Google's UiAutomator
  - can write tests only in Java


# Appium Internal Architecture

Client/Server Architecture



# Code Snippets
```java
//using java client lib (appium)

File f  = new File("src");
File fs = new File(f, "ApiDemos-debug.apk");
		
DesiredCapabilities dCap = new DesiredCapabilities();

dCap.setCapability(MobileCapabilityType.DEVICE_NAME, "Nexus_5X_5.2_API_27");
dCap.setCapability(MobileCapabilityType.APP, fs.getAbsolutePath());
dCap.setCapability(MobileCapabilityType.AUTOMATION_NAME, "uiautomator2");
		
AndroidDriver<AndroidElement> androidDriver = new AndroidDriver<>(new URL("http://127.0.0.1:4723/wd/hub"), dCap);
```		

To identify object/views in an app, you need to depend on (4 locators) xpath, id, classname, uiautomator.

```java
// -------    Using xpath syntax   ---------
"//tagName[@attribute='value']"
Example: 
androidDriver.findElementByXPath("//android.widget.TextView[@text='Preference']").click();


androidDriver.findElementByXPath("(//android.widget.RelativeLayout)[2]").click();
androidDriver.findElementByClassName("android.widget.EditText").sendKeys("Lucky");
androidDriver.findElementsByClassName("android.widget.Button").get(1).click();
```

```java
// ----------    Using Android UI Automator   ------------
//Syntax of Arguments: attribute(value)
driver.findElementByAndroidUIAutomator("text(\"Views\")").click();

// To validate if a View is Clickable
// For properties use "new UiSelector().property(value)"
driver.findElementsByAndroidUIAutomator(new UiSelector().clickable(true)).size();



```