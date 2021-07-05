# Adding Luna Gateway SDK to project

### 1. Download the library

Direct link: https://github.com/indigo-d/luna_android_published_sdk/archive/refs/heads/main.zip

### 2. Copy the library

a) Open your Android Studio project and select `Project` view in the `Project` tool window:

![](https://i.ibb.co/7gHfsGQ/Screenshot-2021-07-05-at-15-52-30.png)

b) Copy downloaded directory `com` inside the `libs` directory:

![](https://i.ibb.co/HGVxwyd/Screenshot-2021-07-05-at-15-57-03.png)

### 3. Load local libraries

Inside **project** scope `build.gradle` file in `allProjects` -> `repositories` add maven with url path to the `libs` directory:

![](https://i.ibb.co/3ynzYjy/Screenshot-2021-07-05-at-15-59-01.png)

### 4. Add dependency

Inside **module** scope `build.gradle` file add dependency to Luna Gateway SDK:

    implementation('com.lunanets:android-gateway-sdk:0.0.140@aar') { transitive = true }

![](https://i.ibb.co/3RrcfGg/Screenshot-2021-07-05-at-16-00-48.png)


## SYNC PROJECT WITH GRADLE FILES


### 5. Provide WorkManager Configuration

To let the app start Gateway SDK automatically after restarting the device, you have to provide WorkManager Configuration through Application extending class *[1]* you can do it in one of two ways:

#### 5.1 Recommended solution

Instead of extending standard android Application you can just extend our Application wrapper called `LunaGatewayApplication`, this way LunaGateway foreground service will be launched automatically at the start of the application as well.  

![](https://i.ibb.co/wYJG2r8/Screenshot-2021-07-05-at-16-05-18.png)

#### 5.2 Standard solution

You can implement `androidx.work.Configuration.Provider` yourself inside your Application class, and provide configuration for example like this:

![](https://i.ibb.co/cy7vNKR/Screenshot-2021-07-05-at-16-07-24.png)

but then you will need to add dependencies for `androidx.work` inside your module scope build.gradle file yourself as well:
		
    implementation "androidx.work:work-runtime-ktx:2.5.0"
    implementation "androidx.work:work-multiprocess:2.5.0"
		
### 6. Requesting foreground location permissions

#### 6.1 Recommended solution

You can just extend one of your activities (preferably your launcher activity) with our `AppCompatActivity` wrapper called `LocationPermissionActivity()`, it will handle both asking user for location permissions and turning on device GPS module if it is not done yet, as well as showing user correct prompts for doing so.

![](https://i.ibb.co/YfWPbSR/Screenshot-2021-07-05-at-16-10-24.png)

#### 6.2 Standard solution

You can request location permissions yourself by following android developer tutorials:

- https://developer.android.com/training/permissions/requesting
- https://developer.android.com/training/location/permissions

### Starting gateway foreground service

One last thing left to do is starting Gateway foreground service so that it will scan for nearby devices (It will be already handled if you used our `LunaGatewayApplication` wrapper from step 5.1).

Inside your Application class `onCreate()` function you just have to call `LunaFinderInitializer.getInstance([application context])` and provide it with your application context:

![](https://i.ibb.co/x2HGpGV/Screenshot-2021-07-05-at-16-13-38.png)

## Luna Gateway set up is finished, now you just have to make sure that your bluetooth is turned on, so that the app can scan correctly for nearby trackers

*[1]* If you don’t know how to add and register Application extending class, here is how it is done
![](https://i.ibb.co/T4S8Ksy/Screenshot-2021-07-05-at-16-32-39.png)

