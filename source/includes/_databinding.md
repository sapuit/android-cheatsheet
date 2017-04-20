Data Binding 
============

## Build Environment

> configure databinding in build.gradle :

```gradle
android {
    ....
    dataBinding {
        enabled = true
    }
}
```

This document explains how to use the Data Binding Library to write declative layout and minimize the glue code necessary to bind your app logic and layouts.

## Data Binding Layout Files

> data binding layout main.xml :

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android">
   <data>
      <variable name="user" type="com.example.User"/>
      <import type="android.view.View"/>
   </data>
   <LinearLayout
       android:orientation="vertical"
       android:layout_width="match_parent"
       android:layout_height="match_parent">
       <TextView android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="@{user.firstName}"
           android:visibility="@{user.isAdult ? View.VISIBLE : View.GONE}"/>
       <TextView android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="@{user.lastName}"/>
        <TextView android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="@{Integer.toString(user.age)}"/>

   </LinearLayout>
</layout>
```

- Start with a root tag of **layout**.
- Followed by **data** element & **view** root element.
- The user **variable** describes a property.
- The attribute properties using the "**@{}**".
- Zero or more **import** elements may be used inside the data element.

## Data Object

> Java object (POJO) for User : 

```java
public class User {
  public final String firstName;
  public final String lastName;
  public final int age;
  public final boolean adult;

  public User(String firstName, String lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }
  public String getFirstName() {
    return this.firstName;
  }
  public String getLastName() {
    return this.lastName;
  }
  public int getAge() {
    return this.age;
  } 
  public boolean isAdult() {
    return this.adult;
  }
}
```

The expression *@{user.firstName}* use for TextView's *android:text* attribute. 

## Binding Data

> MainActivity.java :

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  MainActivityBinding binding = DataBindingUtil.setContentView(this, R.layout.main_activity);
  User user = new User("Sap", "Nguyen", "21");
  binding.setUser(user);
}
```
> Using data binding items inside a ListView or RecyclerView adapter :

```java
ListItemBinding binding = ListItemBinding.inflate(layoutInflater, viewGroup, false);
//or
ListItemBinding binding = DataBindingUtil.inflate(layoutInflater, R.layout.list_item, viewGroup, false);
```

- Binding class (*MainActivityBinding*) will be generated based on the name of layout file (*main_activity.xml*).

## Custom Setter

```xml
<ImageView app:imageUrl="Image_url">
```
```java
@BindingAdapter("imageUrl")
public static void setPaddingLeft(ImageView view, String url) {
    Glide.with(view.getContext()).load(url).placehoder(R.drawable.loading).into(view);
}
```

Reference :

- [Data Binding Library](https://developer.android.com/topic/libraries/data-binding/index.html#data_binding_layout_files).
- [Data binding & MVVM](http://www.slideshare.net/fabio_collini/android-data-binding-in-action-using-mvvm-pattern-droidconuk)