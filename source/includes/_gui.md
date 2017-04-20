# GUI

## Background

### Moving Gradient Background

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