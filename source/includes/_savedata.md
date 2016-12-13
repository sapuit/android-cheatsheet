# Save Data

## Saving Key-Value Sets

> Initialization: 

```java
SharedPreferences pref = getApplicationContext().getSharedPreferences("MyPref", Context.MODE_PRIVATE); // 0 - for private mode
Editor editor = pref.edit();
```

> Storing Data

```java
editor.putBoolean("key_name", true); // Storing boolean - true/false
editor.putString("key_name", "string value"); // Storing string
editor.putInt("key_name", "int value"); // Storing integer
editor.putFloat("key_name", "float value"); // Storing float
editor.putLong("key_name", "long value"); // Storing long
  
editor.commit(); // commit changes
```

> Retrieving Data

```java
pref.getString("key_name", null); // getting String
pref.getInt("key_name", null); // getting Integer
pref.getFloat("key_name", null); // getting Float
pref.getLong("key_name", null); // getting Long
pref.getBoolean("key_name", null); // getting boolean
```

> Clearing or Deleting Data

```java
editor.remove("name"); // will delete key name
editor.remove("email"); // will delete key email
editor.commit(); // commit changes

editor.clear();
editor.commit(); // commit changes
```