# GUI Code sniper

* [Gradient Background Animation](#gradient-background-animation)
* [StatusBar transparent](#statusbar-transparent)
* [Rounded image](#rounded-image)
* [Hide the status bar](#hide-the-status-bar)

## Different values folder for different screens

Folder |Screen size (inch)|Screen size |Qualifier
- | - | - | -
values-sw720dp |10.1” | tablet | 1280x800 mdpi|
values-sw600dp | 7.0”  | tablet | 1024x600 mdpi|
values-sw480dp | 5.4”  | 480x854 | mdpi |
values-sw480dp | 5.1”  | 480x800 | mdpi |
values-xxhdpi| 5.5"  | 1080x1920 | xxhdpi|
values-xxhdpi| 5.5"  | 1440x2560 | xxhdpi|
values-xhdpi | 4.7”   | 1280x720 | xhdpi |
values-xhdpi | 4.65”  | 720x1280 | xhdpi |
values-hdpi | 4.0” | 480x800 | hdpi|
values-hdpi | 3.7” | 480x854 | hdpi|
values-mdpi | 3.2” | 320x480 | mdpi|
values-ldpi | 3.4” | 240x432 | ldpi|
values-ldpi | 3.3” | 240x400 | ldpi|
|values-ldpi | 2.7” | 240x320 | ldpi

## Gradient Background Animation

> main

```java
LinearLayout linearLayout = (LinearLayout) findViewById(R.id.linear_layout);
AnimationDrawable animationDrawable = (AnimationDrawable) linearLayout.getBackground();
animationDrawable.setEnterFadeDuration(2500);
animationDrawable.setExitFadeDuration(5000);
animationDrawable.start();
```

> layout main

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/linear_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@drawable/gradient_animation_list"
    android:orientation="vertical>

    <!--Content goes here-->

</LinearLayout>
```

> gradient_animation_list.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<animation-list xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:drawable="@drawable/gradient_blue"
        android:duration="5000"/>

    <item
        android:drawable="@drawable/gradient_red"
        android:duration="5000"/>

    <item
        android:drawable="@drawable/gradient_teal"
        android:duration="5000"/>

    <item
        android:drawable="@drawable/gradient_purple"
        android:duration="5000"/>

    <item
        android:drawable="@drawable/gradient_indigo"
        android:duration="5000"/>

</animation-list>
```

> gradient_blue.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">

    <gradient
        android:angle="225"
        android:endColor="#1a2980"
        android:startColor="#26d0ce" />

</shape>
```

> gradient_red.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">

    <gradient
        android:endColor="#ff512f"
        android:startColor="#dd2476"
        android:angle="45"/>

</shape>
```


> gradient_teal.xml


```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">

    <gradient
        android:endColor="#614385"
        android:startColor="#516395"
        android:angle="45"/>

</shape>
```
> gradient_purple.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">

     <gradient
        android:endColor="#1d2b64"
        android:startColor="#f8cdda"
        android:angle="135"/>

</shape>
```

> gradient_indigo.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">

 	<gradient
     	android:angle="135"
        android:endColor="#34e89e"
        android:startColor="#0f3443" />

</shape>
```

Reference :
- [Make a moving Gradient Background in Android](http://thetechnocafe.com/make-a-moving-gradient-background-in-android/)
- Gradient combinations : [UiGradients](https://uigradients.com/#Kyoto)

## StatusBar transparent

> Make the status bar traslucent

```xml
<style name="AppTheme" parent="AppTheme.Base">
    <item name="android:windowTranslucentStatus">true</item>
</style>
```

>  A method to find height of the status bar

```java
public int getStatusBarHeight() {
    int result = 0;
    int resourceId = getResources().getIdentifier("status_bar_height", "dimen", "android");
    if (resourceId > 0) 
        result = getResources().getDimensionPixelSize(resourceId);
    return result;
}
```
> MainActivity

```java
Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
setSupportActionBar(toolbar);

toolbar.setPadding(0, getStatusBarHeight(), 0, 0);

// create our manager instance after the content view is set
SystemBarTintManager tintManager = new SystemBarTintManager(this);
// enable status bar tint
tintManager.setStatusBarTintEnabled(true);
// enable navigation bar tint
tintManager.setNavigationBarTintEnabled(true);
// set the transparent color of the status bar, 20% darker
tintManager.setTintColor(Color.parseColor("#20000000"));
``` 

> build.gradle

```xml
compile 'com.readystatesoftware.systembartint:systembartint:1.0.3'
```

## Hide the status bar

```java
// Hide the Status Bar on Android 4.0 and Lower
if (Build.VERSION.SDK_INT < 16) {
    window.setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN,
            WindowManager.LayoutParams.FLAG_FULLSCREEN);
}

setContentView(R.layout.activity_home)

// Hide the status bar on android > 4.0.
val decorView = window.decorView
val uiOptions = View.SYSTEM_UI_FLAG_FULLSCREEN
decorView.systemUiVisibility = uiOptions

setSupportActionBar(toolbar)
```
