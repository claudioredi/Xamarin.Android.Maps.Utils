# Xamarin.Android.Maps.Utils

## Overview
Xamarin (MonoDroid) bindings for [https://github.com/googlemaps/android-maps-utils](https://github.com/googlemaps/android-maps-utils)

Read about Android maps utils on official [Android maps utils website](http://googlemaps.github.io/android-maps-utils/)

Clustering in Xamarin.Android app with customized markers:

<img src="Screenshots/clustering.png?raw=true" title="Clustering in Xamarin.Android app with customized markers" alt="Clustering in Xamarin.Android app with customized markers" width="184">

## Clustered Markers Sample Code

Based on the [official Clustering example from Google](https://developers.google.com/maps/documentation/android-api/utility/marker-clustering)

First, define a MyItem object that inherits from Java.Lang.Object and implements IClusterItem:

```c#

using Com.Google.Maps.Android.Clustering;

...

public class MyItem : Java.Lang.Object, IClusterItem {
		
	private LatLng mPosition;

	public MyItem(double lat, double lng) {
		mPosition = new LatLng(lat, lng);
	}			

	#region IClusterItem implementation

	public LatLng Position
	{
		get
		{
			return mPosition;
		}
	}

	#endregion

}
```

Second, populate the map:

```c#
	// Declare a variable for the cluster manager.
	private ClusterManager mClusterManager;

	private void setUpClusterer(GoogleMap oMap) {

		// Position the map.
		oMap.MoveCamera(CameraUpdateFactory.NewLatLngZoom(new LatLng(51.503186, -0.126446), 10));

		// Initialize the manager with the context and the map.
		// (We're inside a Fragment, so pass in the parent Activity for context)
		mClusterManager = new ClusterManager(Activity, oMap);

		// Point the map's listeners at the listeners implemented by the cluster
		// manager.
		oMap.SetOnCameraChangeListener(mClusterManager);
		oMap.SetOnMarkerClickListener(mClusterManager);

		// Add cluster items (markers) to the cluster manager.
		addItems();
	}

	private void addItems() {

		// Set some lat/lng coordinates to start with.
		double lat = 51.5145160;
		double lng = -0.1270060;

		// Add ten cluster items in close proximity, for purposes of this example.
		for (int i = 0; i < 10; i++) {
			double offset = i / 60d;
			lat = lat + offset;
			lng = lng + offset;
			MyItem offsetItem = new MyItem(lat, lng);
			mClusterManager.AddItem(offsetItem);
		}
	}
```

## Updating Xamarin.GooglePlayServices.Maps

If you want to use this library with another version of Play Services:

Just remove Xamarin.GooglePlayServices.Maps Nuget and references and add your own, like show here:

<img src="Screenshots/using-with-different-versions.png?raw=true" 
title="How to use this library with another version of Play Services" alt="How use this library with another version of Play Services" width="221">

