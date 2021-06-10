##### [back-home](https://mhd22.github.io/all-reading-notes/main-table)

# Read: 42 - Location

<hr>

# Get the last known location

## Create location services client:

* In your activity's `onCreate()` method, create an instance of the Fused Location Provider Client as the following code snippet shows.

```java
private FusedLocationProviderClient fusedLocationClient;

// ..

@Override
protected void onCreate(Bundle savedInstanceState) {
    // ...

    fusedLocationClient = LocationServices.getFusedLocationProviderClient(this);
}
```

* To request the last known location, call the `getLastLocation()` method. The following code snippet illustrates the request and a simple handling of the response:

```java
fusedLocationClient.getLastLocation()
        .addOnSuccessListener(this, new OnSuccessListener<Location>() {
            @Override
            public void onSuccess(Location location) {
                // Got last known location. In some rare situations this can be null.
                if (location != null) {
                    // Logic to handle location object
                }
            }
        });

```


## Location Permission:

You need to add these permissions to your manifest file:

```xml
<manifest ... >
  <!-- To request foreground location access, declare one of these permissions. -->
  <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
  <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
</manifest>

```



<hr>

### This file wrote by [Mohamad Saad Eddin](https://github.com/MHD22).
***you can visit my profile and follow me.❤️😎***
### ______________________________________________


###### Thanks for your time, I hope that you have enjoyed it.

###### [resource1](https://developer.android.com/training/location/retrieve-current)
<!-- ###### [resource2]() -->
<!-- ###### [resource3]() -->
<!-- ###### [resource4]() -->




