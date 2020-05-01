

[![Build Status](https://img.shields.io/badge/platform-android-green)](https://img.shields.io/badge/platform-android-green)
[![Build Status](https://img.shields.io/badge/type-library-blue)](https://img.shields.io/badge/type-library-blue)


# ColourSDK
ColourSDK for Android

* [Features](#-features)
* [Installation](#-installation)
  * [AAR](#using--aar--android-archive)
  * [Adding Dependecies](#adding-dependencies)
* [How to use](#-how-to-use)
* [Interface](#-interfaces)
   * [LivenessListener](#livenesslistener)
* [Customization](#-customization)
  * [Steps](#steps)
  * [Thersholds](#-Thresholds)  
* [Supported OS & SDK Versions](#-supported-os--sdk-versions)

## 🌟 Features
- Supports Android 6.0+


## 📲 Installation
* Requires Java8 support/ gradle plugin 3+

#### Using [ AAR  (Android Archive)](https://developer.android.com/studio/projects/android-library)

An **AAR file** contains a software library used for developing Android apps. It is structurally similar to an . APK **file** (Android Package), but it allows a developer to store a reusable component that can be used across multiple different apps. To integrate AAR  into your android project:
- Purchase SDK license from [facex portal](https://search.facex.io).
- Download the `file.json` config file from the portal. Make sure file name is `file.json`.
- Create `assets` directory in the android project ( if not already there) and copy the downloaded `file.json` to `assets` directory.
- Download the latest `colour.aar` release from [here](https://github.com/teamfacex/LivenessSDK-Android/releases/latest/download/colour.aar).
- Open android studio and add the liveness SDK to your android project
  - Click  `File > New > New Module`.
  - Click `Import .AAR Package` from repo directory then click `Next`.
  -  Enter the location of the `liveness.aar` file in the cloned directory then click `Finish`
- Make sure the library is listed at the top of your `settings.gradle` file, as shown here for a library named "liveness":
  -  `include ':app',  ':colour'`
 - Open the app module's `build.gradle` file and add a new line to the `dependencies` block as shown in the following snippet:
   - `dependencies { implementation project(":colour")  }`

#### Adding Dependencies

- Add OpenCV Repository
  -In build.gradle file for app module add `apply plugin: 'maven'`

  -In build.gradle for project add in `allprojects { repositories {}}`
```
 maven {
            url  "http://dl.bintray.com/steveliles/maven"
}
```  
  
* Add Following dependencies

```
    implementation 'org.opencv:OpenCV-Android:3.1.0'
    implementation 'com.google.android.gms:play-services-vision:20.0.0'

```


## 🐒 How to use
- Make sure to iniialize OpenCV in Activity onResume
#### Kotlin
```
override fun onResume() {
super.onResume()
  if (!OpenCVLoader.initDebug()) {
    Log.e("Opencv", "Internal OpenCV library not found. Using OpenCV Manager for initialization");
      OpenCVLoader.initDebug();
  } else{
      Log.e("Opencv", "OpenCV library found inside package. Using it!");
  }
}
```
#### Java
```
@Override
protected void onResume() {
  super.onResume();
  if (!OpenCVLoader.initDebug()) {
      Log.e("Opencv", "Internal OpenCV library not found. Using OpenCV Manager for initialization");
      OpenCVLoader.initDebug();
    } else {
    Log.e("Opencv", "OpenCV library found inside package. Using it!");
  }
}
```

- Make sure to get the camera permission.
```
if (ContextCompat.checkSelfPermission(
                this,
                Manifest.permission.CAMERA
            ) != PackageManager.PERMISSION_GRANTED
        ) {

            if (ActivityCompat.shouldShowRequestPermissionRationale(
                    this,
                    Manifest.permission.CAMERA
                )
            ) {


            } else {
                ActivityCompat.requestPermissions(
                    this,
                    arrayOf(Manifest.permission.CAMERA),
                    1
                )
            }

        }
override fun onRequestPermissionsResult(requestCode: Int,
                                            permissions: Array<String>, grantResults: IntArray) {
        when (requestCode) {
            1 -> {
                // If request is cancelled, the result arrays are empty.
                if ((grantResults.isNotEmpty() && grantResults[0] == PackageManager.PERMISSION_GRANTED)) {
//                    liveness.startLiveness()
                }
                return
            }

        }
    }
```
#### Kotlin

``` 
import io.facex.liveness.Liveness  
import io.facex.liveness.LivenessListener

class MainActivity : AppCompatActivity(),LivenessListener{
   
private lateinit var liveness: Liveness

 override fun livenessError(live: Boolean?, errorMessage: String?) {   
}  

override fun livenessSuccess(live: Boolean?,bitmap : Bitmap) {  
 
}
override fun onCreate(savedInstanceState: Bundle?) {
	liveness= Liveness(this, R.id.fragment_holder)
	liveness.Eyes=true
	liveness.Mouth=true
	....
	liveness.startLiveness()
  }
}
```

#### Java

```
import io.facex.liveness.Liveness  
import io.facex.liveness.LivenessListener

public class Mainactivity extends AppCompatActivity implements LivenessListener{
  private Liveness liveness;

  @Override
  public void livenessError(Boolean live,String errorMessage){

  }

  @Override
  public void livenessSuccess(Boolean live,Bitmap bitmap){    
    
  }

  @Override
  protected void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);
      setContentView(R.layout.activity_main);
      liveness= new Liveness(this, R.id.fragment_holder);
	    liveness.Eyes=true;
	    liveness.Mouth=true;
	    ....
	    liveness.startLiveness();
   }
}
```

##### layout file for fragment

```
liveness.startLiveness() creates a fragment, so it needs a view to bind the fragment.
Pass in the id of the fragment container using the liveness constructor.
eg: fragment_holder


<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/fragment_holder"
    android:background="@color/black"
    tools:context=".MainActivity">

    .............................
    
</androidx.constraintlayout.widget.ConstraintLayout>
```

## Interfaces

#### LivenessListener 

```
Kotlin

override fun livenessSuccess(live: Boolean?,bitmap : Bitmap) {
  }
  
 override fun livenessError(live: Boolean?, errorMessage: String?) {
  }

Java
  @Override
  public void livenessError(Boolean live,String errorMessage){

  }

  @Override
  public void livenessSuccess(Boolean live,Bitmap bitmap){    
    
  }
```

## 🎛 Customization

You can set some properties for liveness.

#### Steps

|Steps|Value|Default| 
|-------|-------|-------| 
|**Eyes**|`Boolean`|`true`| 
|**Mouth** |`Boolean`|`true`| 
|**Yaw**|`Boolean`|`true`| 
|**Random** |`Boolean`|`true`| 


#### Thresholds

|Property|Values|Default| 
|-------|-------|-------| 
|**EYE_THRESHOLD**|`0.0 ... 6.0`|`2.6`| 
|**MOUTH_THRESHOLD**|`2.0 ... 9.0`|`6.0`| 
|**TIMER**|`Seconds`|`5 seconds`| 
|**MAX_TIMER**|`Seconds`|`15 seconds`|




### 📋 Supported OS & SDK Versions
* Android 6.0+
* Java 8