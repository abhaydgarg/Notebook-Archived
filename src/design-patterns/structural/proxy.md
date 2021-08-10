# Proxy

It lets you provide a substitute for object. A proxy **controls access to the original object.**

Why would you want to control access to an object?

Here is an example: you have a massive object that consumes a vast amount of system resources. You need it from time to time, but not always. In short, a proxy is a object sitting in the middle that is being called by the client to access the real object behind the scenes. In the proxy, extra functionality can be provided, for example caching when operations on the real object are resource intensive, or checking preconditions before operations on the real object are invoked. For the client, usage of a proxy object is similar to using the real object, because both implement the same interface.

## Applicability

There are dozens of ways to utilize the Proxy pattern. Let’s go over the most popular uses.

1. Access control (protection proxy). This is when you want only specific clients to be able to use the service object. The proxy can pass the request to the service object only if the client’s credentials match some criteria.

2. Local execution of a remote service (remote proxy). This is when the service object is located on a remote server. In this case, the proxy passes the client request over the network, handling all of the nasty details of working with the network.

3. Logging requests (logging proxy). This is when you want to keep a history of requests to the service object. The proxy can log each request before passing it to the service.

4. Caching request results (caching proxy). This is when you need to cache results of client requests and manage the life cycle of this cache, especially if results are quite large. The proxy can implement caching for recurring requests that always yield the same results.

## Code

```js
/**
 * 1. RealSubject - Geo Coder to get lat and lon which is
 *    heavy operation and takes much system resources when called each time.
 */

function GeoCoder() {

  this.getLatLng = function(address) {
    // sample data
    if (address === "Amsterdam") {
      return "52.3700° N, 4.8900° E";
    } else if (address === "London") {
      return "51.5171° N, 0.1062° W";
    } else if (address === "Paris") {
      return "48.8742° N, 2.3470° E";
    } else if (address === "Berlin") {
      return "52.5233° N, 13.4127° E";
    } else {
      return "";
    }
  };
}

/**
 * 2. Proxy - The programmer decided to implement a Proxy object
 *    because GeoCoder is relatively slow. It is known that many
 *    repeated requests (for the same location) are coming in.
 *    To speed things up GeoProxy caches frequently requested locations.
 *    If a location is not already cached it goes out to the real GeoCoder
 *    service and stores the results in cache.
 */
function GeoProxy() {
  var geocoder = new GeoCoder();
  var geocache = {};
  var isCache = 'From Cache';

  return {
    getLatLng: function(address) {
      if (!geocache[address]) {
        isCache = 'From Real Subject';
        geocache[address] = geocoder.getLatLng(address);
      } else {
        isCache = 'From Cache';
      }
      console.log(address + ": " + geocache[address] + ' = [' + isCache + ']');
      return geocache[address];
    },
    getCount: function() {
      var count = 0;
      for (var code in geocache) {
        count++;
      }
      return count;
    }
  };
}

/**
 * 3. Client - Several city locations are queried and many of these are
 *    for the same city. GeoProxy builds up its cache while supporting
 *    these calls. At the end GeoProxy< has processed 11 requests but had
 *    to go out to GeoCoder only 3 times. Notice that the client program
 *    has no knowledge about the proxy object (it calls the same interface
 *    with the standard getLatLng method).
 */
function run() {
  var geo = new GeoProxy();

  // geolocation requests

  geo.getLatLng("Paris");
  geo.getLatLng("London");
  geo.getLatLng("London");
  geo.getLatLng("London");
  geo.getLatLng("London");
  geo.getLatLng("Amsterdam");
  geo.getLatLng("Amsterdam");
  geo.getLatLng("Amsterdam");
  geo.getLatLng("Amsterdam");
  geo.getLatLng("London");
  geo.getLatLng("London");

  console.log("\nCache size: " + geo.getCount());
}

/**
 * 4. execute
 */
run();
```
