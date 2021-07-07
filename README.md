# Adding Luna Gateway SDK to project

### 1. Download

a) Inside **project** scope `build.gradle` file in `allProjects` -> `repositories` add path to the repository:

    maven {
            url "https://raw.github.com/indigo-d/luna_android_published_sdk/master"
        }
    }

![](https://i.ibb.co/fDdzVpf/Screenshot-2021-07-07-at-13-39-01.png)

b) Inside **module** scope `build.gradle` file add dependency:

    implementation 'com.lunanets:android-gateway-sdk:0.0.140'

c) **Sync Project with Gradle Files**

![](https://i.ibb.co/yQZBVZ1/Screenshot-2021-07-06-at-14-35-12.png)

### 2. WorkManager Configuration

To start Luna Gateway SDK automatically, you have to provide `WorkManager Configuration` in Application class. It can be done by SDK (recommended solution), or manually.

If the app doesn't have Application class yet, follow the [instuctions](https://docs.rudderstack.com/stream-sources/rudderstack-sdk-integration-guides/rudderstack-android-sdk/add-an-application-class-to-you-android-application).

#### 2.1. Recommended solution

Extend Application class with `LunaGatewayApplication` to launch Luna Gateway SDK foreground service automatically.

![](https://i.ibb.co/wYJG2r8/Screenshot-2021-07-05-at-16-05-18.png)

#### 2.2. Manual solution

<details>
  <summary>Show</summary>

a) Implement `androidx.work.Configuration.Provider` inside Application class, and provide `Configuration` manually:

![](https://i.ibb.co/cy7vNKR/Screenshot-2021-07-05-at-16-07-24.png)

b) Add dependencies for `androidx.work` inside your module scope `build.gradle` file:
		
    implementation "androidx.work:work-runtime-ktx:2.5.0"
    implementation "androidx.work:work-multiprocess:2.5.0"

c) Inside Application class `onCreate()` function call `LunaGatewayInitializer.getInstance([application context])`:

![](https://i.ibb.co/x16XN2L/Screenshot-2021-07-06-at-12-34-03.png)
		
</details>
	
### 3. Request location permission

#### 3.1. Recommended solution

Extend one of the activities (preferably launcher activity) with `AppCompatActivity` wrapper called `LocationPermissionActivity` to handle asking the user for location permission and turning on device GPS module automatically.

![](https://i.ibb.co/YfWPbSR/Screenshot-2021-07-05-at-16-10-24.png)

#### 3.2. Manual solution

<details>
  <summary>Show</summary>

To request **"While using the app"** location permission manually follow the official tutorials:

- https://developer.android.com/training/permissions/requesting
- https://developer.android.com/training/location/permissions
	
</details>
