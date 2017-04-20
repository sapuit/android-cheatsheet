# Code sniper

## Activity

### Activity for result

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