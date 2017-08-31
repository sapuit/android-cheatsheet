Database
========

- [Room database](#room-database)


## Room database

>  project build.gradle

```javascript
repositories {
    jcenter()
    maven { url 'https://maven.google.com' }
}
```

> app build.gradle

```javascript
compile "android.arch.persistence.room:runtime:1.0.0-alpha3"
annotationProcessor "android.arch.persistence.room:compiler:1.0.0-alpha3"
```

> AppDatabase.kt

```javascript
// debug
// adb forward tcp:8080 tcp:8080

@Database(entities = arrayOf(User::class),
        version = 1)
abstract class AppDatabase : RoomDatabase() {

    abstract fun userDao(): UserDao
 

    companion object {

        var INSTANCE: AppDatabase? = null

        fun getAppDatabase(context: Context): AppDatabase? {
            if (INSTANCE == null)
                INSTANCE = Room.databaseBuilder(context, AppDatabase::class.java, "db-app").build()
            return INSTANCE
        }

        fun detroyIntance() {
            INSTANCE = null
        }
    }
}

```

> User.kt

```javascript

@Entity(tableName = "user")
class User(
        @PrimaryKey(autoGenerate = true)
        var id: Long,

        @ColumnInfo(name = "token")
        var token: String = "",

        @ColumnInfo(name = "name")
        var name: String = "",
        
        @ColumnInfo(name = "age")
        var age: Int = "",

        @Embedded
        var address: Address,

        @Ignore
        var  picture: Bitmap
                )

data class Address(val postcode: String,
                   val addressLine1: String)
```

> Book.kt

```javascript
@Entity(foreignKeys = arrayOf(ForeignKey(entity = User::class,
                                  parentColumns = "id",
                                  childColumns = "user_id",
                                  onDelete = ForeignKey.CASCADE))
class Book {
    @PrimaryKey(autoGenerate = true)
    val  bookId: int,

    val title: String= "",

    @ColumnInfo(name = "user_id")
    public int userId;
}                                  
```
> UserDao.kt

```java
    @Dao
    interface UserDao  {

    @Query("SELECT * FROM user")
    fun getAll(): List<User>

    @Query("SELECT * FROM user where name LIKE :arg0")
    fun findByName(name: String): User

    @Query("SELECT * FROM user where id LIKE :arg0")
    fun findById(id: Int): User

    @Query("SELECT COUNT(*) from user")
    fun countUsers(): Int

    @Insert(onConflict = REPLACE)
    fun insert(user: User)

    @Insert
    fun insertAll(vararg users: User)

    @Update
    fun update(user: User)

    @Delete
    fun delete(user: User): Int

    @Query("SELECT * FROM user WHERE age > :arg0")
    fun loadAllUsersOlderThan(minAge: Int): List<User>
}
```

> read data

```javascript
val userDao = AppDatabase.getAppDatabase(context)?.userDao()

object : Thread() {
        override fun run() {
            var user = userDao?.findById(1)
        }
    }.start()
```

> Using live data to update UI

```gradle
compile "android.arch.lifecycle:runtime:1.0.0-alpha1"
compile "android.arch.lifecycle:extensions:1.0.0-alpha1"
```

```java
@Query("SELECT * FROM University")
fun fetchAllData(): LiveData<List<University>>
```

```java
var universityLiveData = sampleDatabase.daoAccess().fetchAllData();
universityLiveData.observe(this, new Observer<List<University>>() {
    @Override
    public void onChanged(@Nullable List<University> universities) {
       //Update your UI here.
    }
})
```
