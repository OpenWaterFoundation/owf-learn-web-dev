# Learn Web Development / Caching

* [Introduction](#introduction)
* [Caching General Goal](#caching-general-goal)
* [Static Website in Local Development Environment](#static-website-in-local-development-environment)
* [Deployed Static Website](#deployed-static-website)
* [Cloud Services](#cloud-services)
* [How Does Web Browser Caching Work?](#how-does-web-browser-caching-work)
* [Options for Controlling Caching](#options-for-controlling-caching)
* [Options for Controlling Caching in Web Services](#options-for-controlling-caching-in-web-services)

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
* Pass the URL of a data file to a JavaScript library, such as Leaflet web mapping URL for a map layer.
* Use the URL of remote resource, such as including in an
[HTML `<iframe>`](https://www.w3schools.com/tags/tag_iframe.asp).
* Use [Ajax](https://www.w3schools.com/xml/ajax_intro.asp) approach and `XmlHttpRequest` function
to read a data file or web service output from the same or different server.
This approach is often encapsulated in library functions that read data.

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
The Chrome browser development tools ***Network*** displays the status of cached resources.

Often, the normal web browser session will cache files, even if they have been changed.
The Chrome "incognito" mode can be used to open a self-contained session that has a clean cache,
and therefore loads changed files.
Although incognito mode is useful to troubleshoot caching,
it is not something that should be recommended for typically users
(they should be able to use their browser normally).

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

## How Does Web Browser Caching Work? ##

Need to document the fundamentals of how browser caching works, using general concepts and specifics
from common web browsers.

* How does the browser check whether a new resource is available?
* Does it rely only on the expiration time for the cached resource?
* Or is there a way to do a two-pass check where the first pass is
to get the header properties and if it is expired go get the full resource?

## Options for Controlling Caching ##

There are multiple options for controlling caching.
The following information is presented in order of simplicity and clarity.
Background that may be useful:

* [Two Simple Rules for HTTP Caching](https://blog.httpwatch.com/2007/12/10/two-simple-rules-for-http-caching/) - need HTML to always load so can determine other resources
* [Strategies for Cache-Busting CSS](https://css-tricks.com/strategies-for-cache-busting-css/) - can also be applied to files other than CSS

### Unique Resource Name ###

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

### Query Parameter on Resource Name ###

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
Instead, the attached version should be dynamically formed, such as by using a random number or date/time.
A random number could theoretically be encountered again, especially if the code logic is not robust.
A date/time could be encountered again if the content is reloaded in a period that is shorter
than the precision of the date/time, so date/time needs to be precise enough to ensure uniqueness.

Common JavaScript libraries often provide a means to format such URLs dynamically through
a `"cache=false"` mechanism, for example:

* **Need to include some common examples here using jQuery, Papa Parse, Leaflet, etc.**

### File Expiration Metadata ###

It is also possible to format file content with metadata to control caching.
**Need to describe here.**

* Whate file types?
* When to use rather than the previous sections.
* HTML `<meta>` tag.
* HTML `<file>` tag?
* Best practices for `Last-Modified` and `ETag`, etc.
* How are images and other files handled?

## Options for Controlling Caching in Web Services ##

This content needs to be completed.

* What are best practices for response headers for web services?
* How does this play with mime types?
* How to use pre-seeded caches to avoid latency.
* How to use caching services such as [memcached](https://memcached.org/).
