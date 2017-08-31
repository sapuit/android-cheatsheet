Retrieving data from API
=========

- [Using Rxjava + Retrofit](#using-rxjava-and-retrofit)
- [Using kotlin library](#using-kotlin-library)


## Using Rxjava and Retrofit

> Build.gradle

```java
// RXJava
compile 'io.reactivex:rxjava:1.2.5'
compile 'io.reactivex:rxandroid:1.2.1'

// retrofit
compile 'com.google.code.gson:gson:2.8.0'
compile "com.squareup.retrofit2:retrofit:$retrofit"
compile "com.squareup.retrofit2:adapter-rxjava:$retrofit"
compile "com.squareup.retrofit2:converter-gson:$retrofit"
compile "com.squareup.okhttp3:okhttp:$okhttp"
compile "com.squareup.okhttp3:logging-interceptor:$okhttp"
```

> manifest

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

> ApiClient.kt

```java
interface ApiClient {
    @FormUrlEncoded
    @POST("login")
    fun login(@Header("app_id") appID: String,
              @Field("phone")  phone: String,
              @Field("password") password: String): Observable<LoginResult>

    @GET("users/{user}/repos")
    fun listRepos(@Path("user") user: String ) : Observable<List<Repo>>;
}
```

> ApiResult.kt
```java
data class LoginResult(@SerializedName("data") val user: User,
                       @SerializedName("message") val message: String,
                       @SerializedName("code") val code: String)
```             

> ApiModule.kt

```java
class ApiModule {

    val apiUrl = "https://demo6117594.mockable.io/"

    val appID = "lkd"

    val service: ApiClient

    companion object {
        val CODE_SUCCESS = "SUCCESS"
        val CODE_ERROR = "ERROR"
    }

    init {
        val logging = HttpLoggingInterceptor()
        logging.level = HttpLoggingInterceptor.Level.BODY

        val httpClient = OkHttpClient.Builder()
        httpClient.addInterceptor(logging)
        httpClient.connectTimeout(15, TimeUnit.SECONDS)
        httpClient.readTimeout(20, TimeUnit.SECONDS)

        val gson = GsonBuilder().setLenient().create()
        val retrofit = Retrofit.Builder()
                .baseUrl(apiUrl)
                .addCallAdapterFactory(RxJavaCallAdapterFactory.create())
                .addConverterFactory(GsonConverterFactory.create(gson))
                .client(httpClient.build())
                .build()

        service = retrofit.create<ApiClient>(ApiClient::class.java)
    }

    fun login(phone: String, pass: String): Observable<LoginResult> {
        return service.login(appID, phone, pass).map { loginResult -> loginResult }
    }
```            

> Using

```java
val progress = indeterminateProgressDialog(getString(R.string.waiting))
ApiModule().login(phone, pass)
            .subscribeOn(Schedulers.io())
            .observeOn(AndroidSchedulers.mainThread())
            .subscribe({ loginResult ->

                if (loginResult.code.equals("SUCCESS")) 
                    saveUserAndStartApp(loginResult.user)

                else if (loginResult.code.equals("ERROR"))
                    toast(loginResult.message)

            }, { e -> e.printStackTrace() }, { progress.dismiss() })
```

## Using kotlin library 

> Entities

```java
data class ForecastList(val city: String, val country: String,
val dailyForecast:List<Forecast>)

data class Forecast(val date: String, val description: String, val high: Int,
val low: Int)

data class ForecastResult(val city: City, val list: List<Forecast>)
```

> API Client
```java
class ForecastRequest(val zipCode: String) {

    companion object {
        private val APP_ID = "15646a06818f61f7b8d7823ca833e1ce"
        private val URL = "http://api.openweathermap.org/data/2.5/" +
        "forecast/daily?mode=json&units=metric&cnt=7"
        private val COMPLETE_URL = "$URL&APPID=$APP_ID&q="
    }

    fun execute(): ForecastResult {
        val forecastJsonStr = URL(COMPLETE_URL + zipCode).readText()
        return Gson().fromJson(forecastJsonStr, ForecastResult::class.java)
    }
}

public interface Command<out T> {
    fun execute(): T
}

class ForecastDataMapper {
    fun convertFromDataModel(forecast: ForecastResult): ForecastList {
        return ForecastList(forecast.city.name, forecast.city.country,    convertForecastListToDomain(forecast.list))
    }
}

class RequestForecastCommand(val zipCode: String) : Command<ForecastList> {
    override fun execute(): ForecastList {
        val forecastRequest = ForecastRequest(zipCode)

        return ForecastDataMapper().convertFromDataModel(
            forecastRequest.execute())
    }
}
```

> Using

```java
async() {
    val result = RequestForecastCommand("94043").execute()
    uiThread { forecastList.adapter = ForecastListAdapter(result) }
}
```