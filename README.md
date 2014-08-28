# GeoFire iOS Module for Titanium #

Appcelerator Titanium module for realtime location queries using [Firebase](http://www.firebase.com).

## Documentation ##

This module is constructed to mimic the official [GeoFire JavaScript Library](https://github.com/firebase/geofire-js) from [Firebase](http://www.firebase.com). All functions available with the official library are also available on this module. All methods take the same arguments and return the same values where applicable.

## Download or Build ##

Download the latest stable build below, or clone the repo build with `./build.py`

- [Download the plug-n-play packaged module here](https://github.com/LeftLaneLab/geofire-titanium/blob/master/iphone/dist/com.leftlanelab.geofire-iphone-0.1.0.zip?raw=true)

## Installing the Module ##

This module is only intended for use with Appcelerator (Titanium) projects. Please follow these instructions to install the packaged module into your environment.

## Using the Module ##

Below is a very simple example of using the GeoFire module to move a `key` around on a map and detect the changes relative to a `radius` with a `center` point.

	// Load the Module
	var GeoFire = require('com.leftlanelab.geofire');
	
	// Create a [GeoFire] Reference from your Firebase
	var geoRef = GeoFire.new('https://l3-appcelerator-demo.firebaseio.com/geofire');
	
	// Set a location for John
	geoRef.set('John', [39.092765, -84.509733]).then(function ()
	{
		console.log('John is Set');
	},
	function (err) {console.log('Something Went Wrong:', err);});
	
	// Create a new [query] with a radius of 2km from a central point
	var query = geoRef.query({
		'center' : [39.103090, -84.512308],
		'radius' : 2
	});
	
	// Attach a simple READY listener to the new [query]
	var queryEventHandle = query.on('ready', function ()
	{
		// ... do something with your UI Here
	
		// Cancel this listener
		queryEventHandle.cancel();
	});
	
	// Add a Listener
	var queryEnterHandle = query.on('key_entered', function (key, location)
	{
		// Update the UI
		lblMsg.text = key + ' Has Arrived';
		lblCoordinates.text = '(' + location[0] + ', ' + location[1] + ')';
	
		// Reveal the MOVE button
		btnInvite.hide();
		btnMove.show();
	});
	
	// Add a Listener
	var queryExitHandle = query.on('key_moved', function (key, location)
	{
		// Update the UI
		lblMsg.text = key + ' Has Moved!';
		lblCoordinates.text = '(' + location[0] + ', ' + location[1] + ')';
	
		// Reveal the KICK button
		btnMove.hide();
		btnKick.show();
	});
	
	// Add a Listener
	var queryExitHandle = query.on('key_exited', function (key, location)
	{
		// Update the UI
		lblMsg.text = key + ' Has Left The Party!';
		lblCoordinates.text = '(' + location[0] + ', ' + location[1] + ')';
	
		// Reveal the INVITE button
		btnKick.hide();
		btnInvite.show();
	});
	
	// Open a Window with a Label & Button
	var winUsers = Ti.UI.createWindow({backgroundColor:'white', layout:'vertical'});
	var lblMsg = Ti.UI.createLabel({top:'50%'});
	var lblCoordinates = Ti.UI.createLabel();
	var btnMove = Ti.UI.createButton({visible:false, title:'Move John'});
	var btnKick = Ti.UI.createButton({visible:false, title:'Kick John'});
	var btnInvite = Ti.UI.createButton({visible:false, title:'Invite John Back'});
	
	btnMove.addEventListener('click', function ()
	{
		// Move John a LITTLE
		geoRef.set('John', [39.092800, -84.509733]);
	});
	
	btnKick.addEventListener('click', function ()
	{
		// Move John a LOT
		geoRef.set('John', [40.092800, -84.509733]);
	});
	
	btnInvite.addEventListener('click', function ()
	{
		// Move John BACK
		geoRef.set('John', [39.092765, -84.509733]);
	});
	
	winUsers.add(lblMsg);
	winUsers.add(lblCoordinates);
	winUsers.add(btnMove);
	winUsers.add(btnKick);
	winUsers.add(btnInvite);
	
	winUsers.open();

## Author ##

This module was developed by [Left Lane Lab](http://www.leftlanelab.com). Please use the [issue tracker](https://github.com/LeftLaneLab/geofire-titanium/issues) for support requests, bug reports, and enhancement ideas... **really, I want to hear from you!**

## License ##

Copyright 2014 Left Lane Lab, LLC.

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at [http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0).
