# Adding Luna Gateway SDK to project

### 1. Download

a) Inside **project** scope `build.gradle` file in `allProjects` -> `repositories` add path to the repository:

    maven {
            url "https://raw.github.com/indigo-d/luna_android_published_sdk/master"
        }
    }

![](https://i.ibb.co/fDdzVpf/Screenshot-2021-07-07-at-13-39-01.png)

b) Inside **module** scope `build.gradle` file add dependency:

    implementation 'com.lunanets:android-gateway-sdk:0.0.197'

c) **Sync Project with Gradle Files**

![](https://i.ibb.co/yQZBVZ1/Screenshot-2021-07-06-at-14-35-12.png)

### 2. WorkManager Configuration

To start Luna Gateway SDK automatically, provide `WorkManager Configuration` in Application class. It can be done automatically by SDK (recommended solution), or manually.

If the app doesn't have Application class yet, follow the [instructions](https://docs.rudderstack.com/stream-sources/rudderstack-sdk-integration-guides/rudderstack-android-sdk/add-an-application-class-to-you-android-application).

#### 2.1. Recommended solution

Extend Application class with `LunaGatewayApplication` to launch Luna Gateway SDK foreground service automatically.

![](https://i.ibb.co/wYJG2r8/Screenshot-2021-07-05-at-16-05-18.png)

#### 2.2. Manual solution

<details>
  <summary>Show</summary>

a) Implement `androidx.work.Configuration.Provider` inside Application class and provide `Configuration` manually:

![](https://i.ibb.co/cy7vNKR/Screenshot-2021-07-05-at-16-07-24.png)

b) Add dependencies for `androidx.work` inside your module scope `build.gradle` file:
		
    implementation "androidx.work:work-runtime-ktx:2.5.0"
    implementation "androidx.work:work-multiprocess:2.5.0"

c) Inside Application class `onCreate()` function call `LunaGatewayInitializer.getInstance([application context])`:

![](https://i.ibb.co/x16XN2L/Screenshot-2021-07-06-at-12-34-03.png)
		
</details>
	
### 3. Request location permission

#### 3.1. Recommended solution

Extend one of the activities (preferably launcher activity) with `AppCompatActivity` wrapper called `LocationPermissionActivity` to handle asking the user for location permission and turning on the device GPS module automatically.

![](https://i.ibb.co/YfWPbSR/Screenshot-2021-07-05-at-16-10-24.png)

#### 3.2. Manual solution

<details>
  <summary>Show</summary>

To request **"While using the app"** location permission manually follow the official tutorials:

- https://developer.android.com/training/permissions/requesting
- https://developer.android.com/training/location/permissions
	
</details>

### 4. Run the app

Check the logs for `Luna Gateway SDK has been started` message. Hurray, the setup is done!

![](https://i.ibb.co/Jn6pGPt/Screenshot-2021-07-07-at-16-51-46.png)


# Understanding LocationPermissionActivity wrapper
## Overview
`LocationPermissionActivity` class is just a wrapper that will make it as easy as possible to use our SDK in your application. You just have to extend your main activity with this class, and it will automatically handle asking user for needed permissions and switching GPS and Bluetooth modules on. Otherwise, it will behave like your standard android `AppCompatActivity`.
## Handling user decisions results
It is possible that while using our `LocationPermissionActivity` wrapper you will want to handle user decisions results yourself, for example show some additional message to the user after he declines granting proper permissions. You can do it easily with the help of our `OnPermissionsActivityResults` class:

![](https://i.ibb.co/ys7J6WY/Screenshot-2021-07-14-at-15-24-17.png)

This class is just a container for the functions that will be called after specific user decisions: 
- Rejecting granting any or all the needed permissions
- Rejecting turning bluetooth module on
- Rejecting turning GPS module on
- Fulfilling all requirements needed for Luna Gateway to work properly

To use this functionality, inside your activity that extends `LocationPermissionActivity` you just have to override **onPermissionActivityResults** field with your own `OnPermissionsActivityResults` implementation:

![](https://i.ibb.co/K2KXcLb/Screenshot-2021-07-14-at-15-31-39.png)

You don't have to specify all the above methods, if you want to use just some of them, inside your implementation provide only those that you need to use. If methods called after rejecting the prompts to turn either bluetooth or location on won't be provided, there will be default persistent android Snackbar shown with message telling user that the Gateway will not work properly without the module that was just being rejected.

![](https://i.ibb.co/SmBvXMm/Screenshot-2021-07-14-at-15-27-27.png)
*Calling `showProperContent()` function after Luna Gateway starts working properly*
