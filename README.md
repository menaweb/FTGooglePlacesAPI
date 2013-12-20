#FTGooglePlacesAPI

**WARNING: This library is not yet officialy released in v1.0+ so the interfaces can still change!**

An open-source iOS Objective-C library for querying [Google Places API][1] using simple callback-blocks based interface.

Perform Google Places API requests with ease in a few lines of code. Library includes, but is not limited to, following:

 - [Place Search][2]
     - Nearby Search (search places withing a specified area)
     - Text Search (search places based on a search string)
 - [Place Details][3] (get more comprehensive information about a place)

##Instalation
 
 1. Implement [AFNetworking 2.0][4]
    - FTGooglePlacesAPI uses AFNetworking 2.0 for all of its networking. Why? Because it rocks!
 2. Download source files from this repository
    - Or use preferred way of GIT submodule ( `git submodule add git@github.com:FuerteInternational/FTGooglePlacesAPI.git` )
 3. Add all files from *FTGooglePlacesAPI* to your project's target

##Usage

You can take a look at the detailed example usage project *XcodeProject/FTGooglePlacesAPI.xcodeproj*. Just be sure to provide your own API key. Or dive in yourself following these few steps...

#### 1. Import FTGooglePlacesAPI files in your implementation file
```objective-c
#import "FTGooglePlacesAPI.h"
```

#### 2. Setup service

In order to communicate with a Google Places API, you must first generate your own API key which you can get at [Google Play Developer Console][5].

You must provide API key to a `FTGooglePlacesAPI` service before making any request using it. Good place for this code is App Delegate.

```objective-c
//  Provide API key to FTGooglePlacesAPIService before making any requests
[FTGooglePlacesAPIService provideAPIKey:@"YOUR_API_KEY"];
```

Optionaly, you can enable debug logging. If this is set to `YES` and when building in debug mode (`#ifdef DEBUG`), service will print some debugging information to the console. In fact there is no need to remove this code even from Release build since the service will not produce any output in non-debug builds.

```objective-c
//  Optionally enable debug mode
[FTGooglePlacesAPIService setDebugLoggingEnabled:YES];
```

#### 3. Create a request

Construct a desired request.

```objective-c
//  Create request searching nearest galleries and museums
FTGooglePlacesAPINearbySearchRequest *request = [[FTGooglePlacesAPINearbySearchRequest alloc] initWithLocationCoordinate:self.locationCoordinate];
request.rankBy = FTGooglePlacesAPIRequestParamRankByDistance;
request.types = @[@"art_gallery", @"museum"];
```

There is a lot of possibilities since the implemented requests objects supports all of the API's functionality. See the example project and the [Google Places API documentation][6].

#### 4. Perform a request and handle the results

```objective-c
//  Execute Google Places API request using FTGooglePlacesAPIService
[FTGooglePlacesAPIService executeSearchRequest:request
                         withCompletionHandler:^(FTGooglePlacesAPISearchResponse *response, NSError *error) {
                             
     //  If error is not nil, request failed and you should handle the error
     if (error)
     {
         // Handle error here
         NSLog(@"Request failed. Error: %@", error);
         
         //  There may be a lot of causes for an error (for example networking error).
         //  If the network communication with Google Places API was successfull,
         //  but the API returned some non-ok status code, NSError will have
         //  FTGooglePlacesAPIErrorDomain domain and status code from
         //  FTGooglePlacesAPIResponseStatus enum
         //  You can inspect error's domain and status code for more detailed info
     }

     //  Everything went fine, we have response object we can process
     NSLog(@"Request succeeded. Response: %@", response);
}];
```

You need to use a proper method based on a request type because the service return will construct the response objects based on a method being called.

Available methods are:

 - `+ (void)executeSearchRequest:withCompletionHandler:` for a both Nearest and Text Search requests
 - `+ (void)executeDetailRequest:withCompletionHandler:` for a Place Detail request

##Compatibility

 - iOS 6+
    - Mainly because of a dependency on AFNetworking 2.0
 - ARC

##Contact

FTGooglePlacesAPI is developed by [FuerteInt](http://fuerteint.com). Please [drop us an email](mailto:open-source@fuerteint.com) to let us know you how you are using this component.

##Contributing and notes

If you like a library, please consider giving it a Github start to let us know it.

Pull requests are very welcome expecting you follow few rules:

 - Document your changes in a code comments and Git commit message
 - Make sure your changes didn't cause any trouble using included example project, unit tests and if appropriate, implement new unit tests for your added functionality

##Version history

#### 0.8
 - First public release on Github, not yet ready for a wide use

##License

Open Source Initiative OSI - The MIT License (MIT):Licensing [OSI Approved License] The MIT License (MIT)

Copyright (c) 2013 Fuerte International

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.


  [1]: https://developers.google.com/places/documentation/
  [2]: https://developers.google.com/places/documentation/search
  [3]: https://developers.google.com/places/documentation/details
  [4]: https://github.com/AFNetworking/AFNetworking
  [5]: https://code.google.com/apis/console
  [6]: https://developers.google.com/places/documentation