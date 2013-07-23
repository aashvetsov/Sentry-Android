# Sentry-Android - Sentry Client for Android
It does what every Sentry client needs to do

Below is an example of how to register Sentry-Android to handle uncaught exceptions

```` java

public class MainActivity extends Activity {

	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		// Sentry will look for uncaught exceptions from previous runs and send them		
		Sentry.init(this, "YOUR-SENTRY-DSN");

	}

}
		
````

## How To Get Started
- Download the [Sentry-Android JAR](https://github.com/joshdholtz/Sentry-Android/raw/master/builds/sentry-0.1.3.jar)
- Download the [Protocol JAR](https://github.com/joshdholtz/Protocol-Android/raw/master/builds/protocol-1.0.4.jar) (Required dependency) - [View more info](https://github.com/joshdholtz/Protocol-Android)
- Place both the JARs in the Android project's "libs" directory
- Code

## This Is How We Do It

### Capture a message
```` java
Sentry.captureMessage("Something significant may have happened");

````

### Capture a caught exception
```` java
try {
	JSONObject obj = new JSONObjet();
} catch (JSONException e) { 
	Sentry.captureException(e);
}

````

### Capture custom event
```` java
Sentry.captureEvent(new Sentry.SentryEventBuilder()
	.setMessage("Being awesome")
	.setCulprit("Josh Holtz")
	.setTimestamp(System.currentTimeMillis())
);

````

### Set a listener to intercept the SentryEventBuilder before each capture
```` java
// Sets a listener to intercept the SentryEventBuilder before 
// each capture to set values that could change state
Sentry.setCaptureListener(new SentryEventCaptureListener() {

	@Override
	public SentryEventBuilder beforeCapture(SentryEventBuilder builder) {
		
		// Needs permission - <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
		ConnectivityManager connManager = (ConnectivityManager) getSystemService(CONNECTIVITY_SERVICE);
		NetworkInfo mWifi = connManager.getNetworkInfo(ConnectivityManager.TYPE_WIFI);

		// Sets extra key if wifi is connected
		try {
			builder.getExtra().put("wifi", String.valueOf(mWifi.isConnected()));
		} catch (JSONException e) {}
		
		return builder;
	}
	
});

````

## Contact

Email: [josh@rokkincat.com](mailto:josh@rokkincat.com)<br/>
Twitter: [@joshdholtz](http://twitter.com/joshdholtz)

## License

Sentry-Android is available under the MIT license. See the LICENSE file for more info.
