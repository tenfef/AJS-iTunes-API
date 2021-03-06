AJS iTunesAPI
=============

AJSiTunesAPI is a simple, asynchronous Cocoa API controller and object for searching the iTunes Search API on either Mac OSX or iOS projects.

This library is a work-in-progress, but current features should work as expected. Support for iTunes lookups from artist ID is omitted, and planned.

### Requirements

AJSiTunesAPI uses blocks for API response callbacks, therefore iOS 4 or greater (or OSX 10.6) is required by your project to use the library. If you need iOS 3.x support, this library is not for you.

### Dependencies

AJSiTunesAPI has dependencies on the following, linked via git submodule

* ASIHTTPRequest (for making and sending HTTP requests, this in turn has some framework dependancies)
* SBJSON (for parsing API responses)
* ISO8601 Date Parser (for parsing of dates, funny that...)

### Documentation

For a rundown of the type of results to expect, source documentation of the API can be found here: http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html

Results are parsed to Cocoa objects, with properties for most of the returned data. Check the header files for data types and properties supported.

Installation
============

Installation is fairly straight forward, clone the project out, using the recursive flag to grab all submodules:

`git clone --recursive git://github.com/andysmart/AJS-iTunes-API.git`

The just copy the `External` and `Classes` directories into your project, and include the `#import "AJSiTunesAPI.h"` header in your classes.

Usage
=====

AJSiTunesAPI can be used in two ways, either using the delegate callbacks, or the block based API.

### Delegate

There are two delegate callbacks for AJSiTunesAPI, for completion and failure. Completion returns an NSArray of AJSiTunesResult objects, and failure returns a NSError containing failure details.

	self.api = [[[AJSiTunesAPI alloc] init] autorelease];
	self.api.delegate = self;
	
	//Simple search, for all types and limited to 50 / US country code
	[self.api searchMediaWithSearchTerm:@"Jack Johnson"];
	
	//More refined search, passing media types, limit and country
	[self.api searchMediaWithType:kAJSiTunesAPIMediaTypeMusic searchTerm:@"Jack Johnson" countryCode:@"US" limit:10];
	
### Block Based

You can optionally pass two blocks for handling responses inline, if both a block and a delegate is set only the block callback will fire. The block structures are defined in `AJSiTunesAPI.h`.

	self.api = [[[AJSiTunesAPI alloc] init] autorelease];
	
	[self.api searchMediaWithSearchTerm:@"Jack Johnson" completionBlock:^(NSArray *results) { //Completion handler } failureBlock:^(NSError *error) { //Error handler here }];
	
Example
=======

Included in the source is an example tableView controller, using AJSiTunesAPI to fetch results and display them (example uses block callbacks).

Credits
=======

[ASIHTTPRequest](https://github.com/pokeb/asi-http-request)
[SBJSON](https://github.com/stig/json-framework)
[ISO8601 Parser](http://boredzo.org/iso8601parser/)