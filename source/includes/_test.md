Test & Debug
============

## Test

Resource:

- [Unit tests with Mockito - Tutorial](http://www.vogella.com/tutorials/Mockito/article.html)
- [Building Effective Unit Tests](https://developer.android.com/training/testing/unit-testing/index.html)
- [Testing UI for a Single App](https://developer.android.com/training/testing/ui-testing/espresso-testing.html)
- [Android testing: Unit testing ](http://alexzh.com/tutorials/android-testing-unit-testing/)
- [Mockito and Robolectric (part 2)](http://alexzh.com/tutorials/android-testing-mockito-robolectric/)

## Debug 

### Debug code

> Debug in a class

```java
private static final String TAG = MainActivity.class.getSimpleName();
private static final boolean isDebug = true;

private void log(String s) {
    if(isDebug)
    	Log.d(TAG, s);
}
```

