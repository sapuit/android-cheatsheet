Location
===========

- [Get current location using GPS](#get-current-location)

## Get current location

> build.gradle

```gradle
dependencies {
    // for emulator
    compile 'com.google.android.gms:play-services:8+'
    compile 'com.google.android.gms:play-services-location:8+'

    // for real device
    //compile 'com.google.android.gms:play-services:10.2.1'
}
```

> AndroidManifest.xml

```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
```

> LocationHelper.kt

```javascript
class LocationHelper(private var context: Context? = null,
                     private var onLocationListenner: (lat: Double, long: Double) -> 
                     Unit) : LocationListener, GoogleApiClient.ConnectionCallbacks {

    private var gac: GoogleApiClient? = null
    private var locationRequest: LocationRequest? = null

    override fun onLocationChanged(location: Location?) {
        if (location != null) {
            onLocationListenner(location.latitude, location.longitude)
            gac?.disconnect()
        }
    }

    override fun onConnected(p0: Bundle?) {
        LocationServices
            .FusedLocationApi
            .requestLocationUpdates(gac,locationRequest, this)
    }

    override fun onConnectionSuspended(p0: Int) {}

    init {
        if (locationRequest == null) {
            locationRequest = LocationRequest()
            locationRequest?.interval = 2 * 1000
            locationRequest?.fastestInterval = 2000
            locationRequest?.priority = LocationRequest.PRIORITY_HIGH_ACCURACY
        }

        if (gac == null)
            gac = GoogleApiClient.Builder(context!!)
                    .addConnectionCallbacks(this)
                    .addApi(LocationServices.API)
                    .build()
    }

    fun getLocation() {
        if (NetworkHelper.checkLocationPermission(context!!)
                && NetworkHelper.isGoogleApiAvailable(context!!)
                && NetworkHelper.isGPSEnabled(context!!)) {
            gac?.connect()
        }
    }

    fun detroy() {
        context = null
        gac = null
        locationRequest = null
    }
}

```