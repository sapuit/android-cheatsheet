# Code sniper

* [FolderHelper](#folderhelper)
* [Activity for result](#activity-for-result)
* [Android Login and Registration](#)
* [Clear Activity Stack](#clear-activity-stack)


## FolderHelper

```kotlin
val ROOT_FOLDER = Environment.getExternalStorageDirectory().absolutePath + File.separator + GlobalParams.APP_NAME + File.separator
val PHOTO_FOLDER = ROOT_FOLDER + "photo" + File.separator
val QRCODE_FOLDER = ROOT_FOLDER + "qrcode" + File.separator

private val TAG = this::class.java.simpleName
private val isDebug = false
private fun log(msg: String) {
    if (isDebug) log(msg)
}

fun creatAppFolder() {
    try {
        createFolder(ROOT_FOLDER)
        createFolder(PHOTO_FOLDER)
        createFolder(QRCODE_FOLDER)

    } catch (e: Exception) {
        e.printStackTrace()
        //toast(getString(R.string.not_create_app_folder))
    }
}

private fun createFolder(pathFolder: String){
    val folder = File(pathFolder)
    if (!folder.exists()) {
        folder.mkdirs()
        log("Create ${folder.absolutePath} successfully")

        val nomedia = File(QRCODE_FOLDER + ".nomedia")
        if (!nomedia.exists()) {
            try {
                nomedia.createNewFile()
                log("Create ${nomedia.path} successfully")
            } catch (e: IOException) {
                log("Create ${nomedia.path} fail")
                e.printStackTrace()
            }
        }
    }
}
```


## Activity for result
    
> FirstActivity call the SecondActivity using  startActivityForResult()

```java
Intent i = new Intent(this, SecondActivity.class);
startActivityForResult(i, 1);
```

> In secondActivity if you want send back data

```java
Intent returnIntent = new Intent();
returnIntent.putExtra("result",result);
setResult(Activity.RESULT_OK,returnIntent);
finish();
```

> If you don't want to return data:

```java
Intent returnIntent = new Intent();
setResult(Activity.RESULT_CANCELED, returnIntent);
finish();
```
> When the user hits the back button set the resultCode as Activity.RESULT_CANCELED to indicate a failure

```java
 @Override
    public void onBackPressed() {
        setResult(Activity.RESULT_CANCELED);
        super.onBackPressed();
    }
```

> Now in your FirstActivity class write following code for the onActivityResult() method.

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    if (requestCode == 1) {
        if(resultCode == Activity.RESULT_OK){
            String result=data.getStringExtra("result");
        }
        if (resultCode == Activity.RESULT_CANCELED) {
            //Write your code if there's no result
        }
    }
}//onActivityResult
```

## Splash

## Session Manager

```java
public class SessionManager {
    // LogCat tag
    private static String TAG = SessionManager.class.getSimpleName();
 
    // Shared Preferences
    SharedPreferences pref;
 
    Editor editor;
    Context _context;
 
    // Shared pref mode
    int PRIVATE_MODE = 0;
 
    // Shared preferences file name
    private static final String PREF_NAME = "AndroidHiveLogin";
     
    private static final String KEY_IS_LOGGEDIN = "isLoggedIn";
 
    public SessionManager(Context context) {
        this._context = context;
        pref = _context.getSharedPreferences(PREF_NAME, PRIVATE_MODE);
        editor = pref.edit();
    }
 
    public void setLogin(boolean isLoggedIn) {
 
        editor.putBoolean(KEY_IS_LOGGEDIN, isLoggedIn);
 
        // commit changes
        editor.commit();
 
        Log.d(TAG, "User login session modified!");
    }
     
    public boolean isLoggedIn(){
        return pref.getBoolean(KEY_IS_LOGGEDIN, false);
    }
}
```

## Android Login and Registration

## Clear Activity Stack

```java
 intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_CLEAR_TASK);
```


