# Learn Web Development / Caching

* [Introduction](#introduction)
* [Caching General Goal](#caching-general-goal)
* [Static Website in Local Development Environment](#static-website-in-local-development-environment)
* [Deployed Static Website](#deployed-static-website)
* [Cloud Services](#cloud-services)
* [How Does Web Browser Caching Work?](#how-does-web-browser-caching-work)
* [Options for Controlling Caching](#options-for-controlling-caching)
* [Options for Controlling Caching in Web Services](#options-for-controlling-caching-in-web-services---needs-further-research)
* [Resources](#resources)

-------------

## Introduction ##

Caching is an approach that is used to improve the user experience and
increase efficiency in using computing resources.
The basic concept is that when a resource such as web page content file or data resource needs to be used,
only retrieve the resource from the original source if a new version is available.
Otherwise, use a cached (local copy) directly because the previous version has not changed.

Caching is generally implemented in consuming applications such as web browsers,
but not software that directly accesses data, such as file transfer tools like `ftp` and `curl`.
Caching can be implemented at multiple levels, for example in database server, web server,
web proxy server, web browser, although this documentation focuses on web browser caching.

Caching is generally good when it improves performance.  However, there are also trade-offs.
For example, cached resources take hard-disk and memory space on the local device.
Therefore, the cache will fill up at some point and cached resources will be recycled.
Perhaps more important is to ensure that the caching mechanism does not result in
a user of information seeing old cached versions when there are new versions available.
For example, the developers of a web application may release a new software version or new content
and users should be able to see such updates.

The control of caching features in software
can be controlled on the publishing and receiving end.
For example, publishers of content can force caching to be disabled for some resources,
for example, if those resources are expected to change very frequently.
The control of caching should initially be configured by the publisher of content because
they know more about the content.
Default caching settings in web browsers may or may not be appropriate
but in general asking users to change such things is probably inconvenient and may work against norms.

The following sections present several common cases that illustrate caching issues.
Other sections describe technical details involved in overcoming caching issues.

## Caching General Goal ##

The general goal of caching is to use a local copy of a resource first if it makes sense to do so.
This minimizes computational resources such as internet travel-time, performance time,
cost of cloud services, etc.

* If the original version of a resource is newer than the cached version,
the web application should retrieve the newer remote resource and use it.
* The expiration time of published resources should be accurately represented so that the time
can be used by caching features.
* Behavior of software such as web browsers should automatically and appropriately
handle caching so that the user experience is "seamless".
For example, deployment of new static website content or web services data should
be recognized by the web browser without having to refresh a browser page, run in "incognito mode",
manually clear the cache, etc.

## Static Website in Local Development Environment ##

Development of web applications typically involves using a local development environment
"sand box" where the application runs using a limited environment.
The more complex the software, the more complex the development environment needs to be.
A simple example is to run a simple http server, such as Python server,
to serve the files from the local file system,
and view with a web browser using address such as `http://localhost:8000/index.html`,
in this case using port 8000 so as to minimize likelihood of conflicts with software using port 8000.

A simple static website application (no server application other than web server for website files)
may involve only HTML, CSS, JavaScript, and data files, such as
comma-separated-value data and GeoJSON files visualization.
The web application may access data resources in several ways:

* Use the URL for a data file directly, such as a relative path URL with files found in the website file tree.   
```html 
<link rel="stylesheet" href="relative/file/path.css" />
<script src="relative/file/path.js"></script>
```
* Pass the URL of a data file to a JavaScript library, such as Leaflet web mapping URL for a map layer.
```html
<script>
  L.tileLayer('https://api.tiles.mapbox.com/data/file', {}).addTo(mymap);
</script>
```
* Use the URL of remote resource, such as including in an
[HTML `<iframe>`](https://www.w3schools.com/tags/tag_iframe.asp).
```html
<iframe data-src="http://viz.openwaterfoundation.org/co/owf-viz-co-snodas-gapminder/" scrolling="no" frameborder="0"></iframe>
```
* Use [Ajax](https://www.w3schools.com/xml/ajax_intro.asp) approach and `XmlHttpRequest` function
to read a data file or web service output from the same or different server.
This approach is often encapsulated in library functions that read data.
```javascript
$(document).ready(function() {
    $.ajax({
        type: "GET",
        url: "data.txt",
        dataType: "text",
        success: function(data) {processData(data);}
     });
});
```

In all cases, the web developer must be able to effectively test the application during development
with frequent changes to source files and data.
Confusion about which version of code and data are used can waste time during development.

The development environment typically includes a way to run a local web server to serve the application files.
If the application uses only core technologies (HTML, CSS, JavaScript), then it is convenient to
serve the application using a simple web server, such as Python
(see: [Big list of http static server one-liners](https://gist.github.com/willurd/5720255)).

In a local development website, such as a single-page web application, the `index.html` top-level page is loaded,
which contains `<script>` elements to import JavaScript libraries, CSS files, CSV data files, and other content.
The `index.html` file is not cached, but other files are often cached.
The web browser tracks filenames and content types and makes decisions about what to cache.
The Chrome browser development tools **`Network`** displays the status of cached resources.

Often, the normal web browser session will cache files, even if they have been changed.
The Chrome "incognito" mode can be used to open a self-contained session that has a clean cache,
and therefore loads changed files.
Although incognito mode is useful to troubleshoot caching,
it is not something that should be recommended for typical users
(they should be able to use their browser normally).

Another option for clearing the cache in a development environment is to do a hard refresh on the page. 
To do this open developer tools (right-click and select **`inspect`**, or press **`F12`**), 
then right click on the Reload button and select **`Empty Cache and Hard Reload`**, or if using 
Windows or Linux and not opening developer tools, hold down **`ctrl`** and press **`F5`** on the keyboard. 
On a Mac Hold **`⇧ Shift`** and click the Reload button.

![right click refresh](https://www.getfilecloud.com/blog/wp-content/uploads/2015/03/Hardrefresh-chrome.png)

See [Options for Controlling Caching](#options-for-controlling-caching) to control caching behavior.

## Deployed Static Website ##

A static website application that is developed similar to the previous case can be deployed to the web
with little or no modification,
for example to an Amazon S3 static website or other content delivery network (CDN).
For example, copy the files to an Amazon S3 bucket location that is configured as a public static website.

See [Options for Controlling Caching](#options-for-controlling-caching) to control caching behavior.

Because the website deployment often uses an upload script,
it is possible to dynamically change filenames to add a version.
For example, it may be desirable to retain the names of software files in version control
rather than changing in source code.

## Cloud Services ##

Need to research whether cloud services like Amazon S3 have ways to configure rules
for server-side caching.

A possible solution might be found using [Amazon ElastiCache](https://aws.amazon.com/elasticache/)

## How Does Web Browser Caching Work? ##

Web Caching is storing of HTTP responses temporarily for fast retrieval later on. Web Caching helps to
optimize the user experience by using less bandwidth and reducing web server load time. Systems that 
use web caching include Search Engines, Web Browsers, Content Delivery Networks and Web Proxies.

The caching mechanism of these systems can be [controlled](#options-for-controlling-caching)
using caching meta tags or HTTP caching headers.<sup>[1](#user-content-f1)</sup>

**Expiration Time for Caching:**  
Once a resource is stored in a cache, it could theoretically be served by the cache forever. 
Caches have finite storage so items are periodically removed from storage. This process is 
called cache eviction. On the other side, some resources may change on the server so the cache 
should be updated. As HTTP is a client-server protocol, servers can't contact caches and clients 
when a resource changes; they have to communicate an expiration time for the resource. Before 
this expiration time, the resource is fresh; after the expiration time, the resource is stale. 
Eviction algorithms often privilege fresh resources over stale resources. Note that a stale 
resource is not evicted or ignored; when the cache receives a request for a stale resource, 
it forwards this request with a `If-None-Match` to check if it is in fact still fresh. 
If so, the server returns a `304` (Not Modified) header without sending the body of the requested 
resource, saving some bandwidth. 
<sup>[2](#user-content-f2)</sup>

**`expirationTime = responseTime + freshnessLifetime – currentAge`**

The **`freshnessLifetime`** is calculated based on several headers. If a `Cache-control: max-age=N` header 
is specified, then the freshness lifetime is equal to N. If this header is not present, which is very 
often the case, then we look for an `Expires` header. If an `Expires` header exists, then its value minus 
the value of the “Date” header determines 
the freshness lifetime. Finally, if neither header is present, then we look for a `Last-Modified` header. 
If this header is present, then the cache’s freshness lifetime is equal to the value of the `Date` header 
minus the value of the `Last-modified` header divided by 10. If none of this headers are there then the 
response is not cached.

**`responseTime`** is the time at which the response was received according to the client.

The **`currentAge`** is usually close to zero, but is influenced by the presence of an `Age` header, 
which proxy caches may add to indicate the length of time a document has been sitting in its cache. 
The precise algorithm, which attempts avoid error resulting from clock skew, 
is described in [RFC 2616 section 13.2.3](https://tools.ietf.org/html/rfc2616#section-13.2.3). 
<sup>[1](#user-content-f1)</sup>

See [How Web Caching Works](http://qnimate.com/all-about-web-caching/), 
[HTTP Caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching).

## Options for Controlling Caching ##

There are multiple options for controlling caching.
The following information is presented in order of simplicity and clarity.
Background that may be useful:

* [Two Simple Rules for HTTP Caching](https://blog.httpwatch.com/2007/12/10/two-simple-rules-for-http-caching/) - need HTML to always load so can determine other resources
* [Strategies for Cache-Busting CSS](https://css-tricks.com/strategies-for-cache-busting-css/) - can also be applied to files other than CSS

### Cache-Control Headers/Expires Headers (Meta Tags) ###
Cache-Control was introduced in HTTP 1.1 whereas Expires header has introduced in HTTP 1.0. 
So we must use both of them for better support of clients.

#### Cacheability: ####
* **`public`** - Indicates that the response may be cached by any cache. Gives a greater performance gain and a much 
greater scalability gain. Client may receive cached copies, never having obtained the oirignal directly from 
the server. 
* **`private`** - Indicates that the response is intended for a single user (the client that the IP was created for) 
and must not be stored by a shared (proxy server) cache. Generally this applies to a cache maintained by that client 
itself.
* **`no-cache`** - Systems will cache the response, but this forces caches to submit the request to the origin
server for validation before releasing a cached copy.
* **`only-if-cached`** - Indicates to not retrieve new data. This being the case, the server wishes the client to obtain
a response only once and then cache. From this moment the client should keep releasing a cached copy and avoid
contacting the origin-server to see if a newer copy exists.
<sup>[1](#user-content-f1)</sup> <sup>[2](#user-content-f2)</sup> <sup>[3](#user-content-f3)</sup>

#### Expiration: ####
* **`max-age=<seconds>`** - Specifies in seconds the maximum amount of time a resource will be considered fresh. 
Contrary to `Expires`, this directive is relative to the time of the request. After the response has expired 
it's deleted from the cache.
* **`s-maxage=<seconds>`** - Overrides `max-age` or the `Expires` header, but it only applites to shared caches 
(e.g. **proxies**) and is ingored by a private cache.
* **`max-stale[=<seconds>]`** - Indicates that the client is willing to accept a response that has exceeded 
its expiration time. Optionally, you can assign a value in seconds, indidcating the time the response must not be 
expired by.
* **`min-fresh=<seconds>`** - Indicates that the client wants a response that will still be fresh at least 
the specified number of seconds.
<sup>[1](#user-content-f1)</sup> <sup>[2](#user-content-f2)</sup>

#### Revalidation and Reloading: ####
* **`must-revalidate`** - The cache must verify the status of the stale resources before using it and expired ones 
should not be used.
* **`proxy-revalidate`** - Same as `must-revalidate`, but it only applies to shared caches (e.g. **proxies**) 
and is ignored by a private cache.
* **`immutable`** - Indicates that the response body will not change over time. The resource, if unexpired, 
is unchanged on the server and therefore the client should not send a conditional revalidation for it 
(e.g. `If-None-Match` or `If-Modified-Since`) to check for updates, even when the user explicitly refreshes 
the page. clinets that aren't aware of this extension must ignore them as per teh HTTP specification. In 
Firefox, `immutable` is only honored on `https://` transactions. For more informaiton, see also this 
[blog post](http://bitsup.blogspot.com/2016/05/cache-control-immutable.html).
<sup>[2](#user-content-f2)</sup>

#### Other: ####
* **`no-store`** - The cache should not store anything about the client request or server response.
* **`no-transform`** - No transformations or conversions should be made to the resource. The 
Content-Encoding, Content-Range, Content-Type headers must not be modified by a proxy. A non-
transparent proxy might, for example, convert between image formats in order to save cache space 
or to reduce the amount of traffic on a slow link. The `no-transform` directive disallows this.
<sup>[2](#user-content-f2)</sup>

#### Examples: ####
Expires sets a future date and time that the response will be cached until. If assigned a past 
time or `-1` then these systems do not cache the response. Here these systems will not cahce 
the response:
```
Expires:-1
Cache-Control: no-store
```
This would be entered into an `.html` file as:
```html
<meta http-equiv="Expires" content="-1"/>
<meta http-equiv="Cache-Control" content="no-store"/>
```

Here these systems will cache the response but before serving the response the client will try to 
revalidate but as we didn’t provide Last-Modified header, client will send revalidation request 
without If-Modified-Since and therefore server will response with 200 status code which is 
refetching the page again.
```
Expires: Thu, 15 Aug 2060 09:00:00 GMT
Cache-Control: no-cache, must-revalidate, expires=360000000
```

Here browser will cache the document till 15 Aug 2015 09:00:00. Client will not revalidate the 
cache before serving.
```
Expires: Thu, 15 Aug 2015 09:00:00 GMT
Last-Modified: Thu, 15 Aug 2011 09:00:00 GMT
```

### Unique Resource Name (Cache-Busting) ###

The simplest way to ensure proper caching is to name resources with a unique name.
For example, if using a JavaScript library and its accompanying CSS file, make sure to name
the file with an appropriate version.  For example, use `jquery-3.3.1.min.js` for jQuery, not `jquery.min.js`.
The resource names will then change only when they are actually updated,
such as when a coordinated update of a website occurs.

A variation on this approach is that if it is desirable to retain the same name for a resource,
such as `my.css`, for example to keep the same name in a code repository,
then the resource can be renamed at deployment time.
For example, add a version to the files based on the date and
search and replace references using a script as the file is uploaded to the cloud or an installer is created.
Deployment tools in development environments may have options to do this.
Because the resource name does not change in a non-deployed local development environment,
it may be necessary to use Chrome incognito mode (or similar tool) during development.

### Query Parameter on Resource Name (Cache-Busting) ###

Changing the resource name by changing the resource filename may not be possible in all cases.
For example, an application may rely on a dynamic data file that is read using an Ajax approach.
Changing the resource name would require changing the source code many times.

A trick that can be used to disable caching on a resource is to attach a query parameter
to the resource address.  This works even if it is a simple file such as a comma-separated-value (CSV) file.
For example, see the `?ver=1` in the following syntax:

```
http://some-server/some-path/data-filename.csv?ver=1
```

However, the above is really no better than adding a version number because the URL is static.
Instead, the attached version should be dynamically formed, such as by using a random number or date/time 
(see example below).
A random number could theoretically be encountered again, especially if the code logic is not robust.
A date/time could be encountered again if the content is reloaded in a period that is shorter
than the precision of the date/time, so date/time needs to be precise enough to ensure uniqueness.

#### Examples: ####
Common JavaScript libraries often provide a means to format such URLs dynamically through
a `"cache=false"` mechanism, for example:
```javascript
$.ajax({
  url:"path/to/file",
  cache: false,
  success:function(data){
    //do something
  }
});
```
Cache-busting using Leaflet or Papaparse and `new Date().getTime()`:
```javascript
// Leaflet MapBox
var mymap = L.map('mapbox').setView([40.5853, -105.0844], 13);

L.tileLayer('https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token={accessToken}&{dateTime}', {
    attribution: 'Map data &copy; <a href="http://openstreetmap.org">OpenStreetMap</a> contributors, <a href="http://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="http://mapbox.com">Mapbox</a>',
    maxZoom: 18,
    id: 'mapbox.streets',
    accessToken: 'your access token',
    dateTime:  new Date().getTime()
}).addTo(mymap);

```
```javascript
//process csv file
Papa.parse('stories.csv?=' + new Date().getTime(), {
  header:true,
  error:function(error){
    throw new Error;
  },
  complete:function(results){
    //do something here
  }
});
```

### ETags ###

The `ETag` HTTP response header is an identifier for a specific version of a resource. It allows
caches to be more efficient, and saves bandwidth, as a web server does not need to send a full
response if the content has not changed. On the other side, if the content has changed, etags
are useful to help prevent simultaneous updates of a resource from overwriting each other
("mid-air collisions").

If the resource at a given URL changes, a new `ETag` value must be generated. Etags are therefore
similar to fingerprints and might also be used for tracking purposes by some servers. A comparison
of them allows to quickly determine whether two representations of a resource are the same,
but they might also be set to persist indefinitely by a tracking server. 
<sup>[4](#user-content-f4)</sup>

**Syntax:**
```
ETag: W/"<etag_value>"
ETag: "<etag_value>"
```

**Directives:**
* **`W/`** - (optional) `'W/'` (case-sensitive) indicates that a weak validator is used. Weak 
validators are easy to generate but are far less useful for comparisons. Strong validators are 
ideal for comparisons but can be very difficult to generate efficiently. Weak Etag values of 
two representations of the same resources might be semantically equivalent, but not byte-for-byte 
identical.
* **`<etag_value>`** - Entity tags uniquely representing the requested resources. They are a 
string of ASCII characters placed between double quotes (Like `"675af34563dc-tr34"`). The method 
by which `ETag` values are generated is not specified. Oftentimes, a hash of the content, a hash 
of the last modification timestamp, or just a revision number is used. For example, MDN uses a 
hash of hexadecimal digits of the wiki content.
<sup>[4](#user-content-f4)</sup>

See this [Stackoverflow](https://stackoverflow.com/questions/162105/whats-the-best-way-to-create-an-etag) 
for different advice on creating an ETag.

See also [ETag - HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag).

### Meta Tags, ETags, or Cache-Busting? ###

From research online there seems to be a fair amount of combining some meta tags with cache 
busting techniques. 
Meta tags offer the first level of specifying how and when resources should be reloaded and 
when to discard what lies in the cache. To make sure different resources are being reloaded 
use cache busting techniques, such as adding file name extensions or editing the file names 
to include random data. Between these two techniques there can be a fair level of certainty
that resources will be reloaded appropriately.

From this [article](https://css-tricks.com/strategies-for-cache-busting-css/) it seems that 
using ETags may not be the best solution in general. There is a quote from Yahoo which [says](https://developer.yahoo.com/performance/rules.html):

>The problem with ETags is that they typically are constructed using attributes that make them 
unique to a specific server hosting a site. ETags won't match when a browser gets the original 
component from one server and later tries to validate that component on a different server, a 
situation that is all too common on Web sites that use a cluster of servers to handle requests.

### File Expiration Metadata - Needs Further Research ###

It is also possible to format file content with metadata to control caching.
**Need to describe here.**

* What file types?
* HTML `<file>` tag?
* How are images and other files handled?

## Options for Controlling Caching in Web Services - Needs Further Research ##

This content needs to be completed.

* What are best practices for response headers for web services?
* How does this play with mime types?
* How to use pre-seeded caches to avoid latency.
* How to use caching services such as [memcached](https://memcached.org/).

## Resources ##
Much of the information provided in this documentation is <a id="helloWorld">compiled</a> from the sources below:

<a id="f1">[1]:</a> [How Web Caching Works](http://qnimate.com/all-about-web-caching/)   
<a id="f2">[2]:</a> [HTTP Caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)  
<a id="f3">[3]:</a> [Private vs Public in Cache-Control (stackoverflow)](https://stackoverflow.com/questions/3492319/private-vs-public-in-cache-control)  
<a id="f4">[4]:</a> [ETag - HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag)

Extra reading:
* [AWS Caching Overview](https://aws.amazon.com/caching/)    
* [A Web Developers Guide to Caching](https://medium.com/@codebyamir/a-web-developers-guide-to-browser-caching-cc41f3b73e7c) 
* [Web Caching Basics: Terminology, HTTP Headers, and Caching Strategies](https://www.digitalocean.com/community/tutorials/web-caching-basics-terminology-http-headers-and-caching-strategies)  
* [Strategies for Cache-Bustin CSS](https://css-tricks.com/strategies-for-cache-busting-css/)
