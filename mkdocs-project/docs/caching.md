# Learn Web Development / Caching

Caching is an approach that is used to improve overall performance and
efficient use of computing resources.
The basic concept is that when a resource such as web page content file or data resource needs to be used,
only retrieve the resource from the original source if a new version is available.
Otherwise, use a cached (local copy) directly because the previous version has not changed.

Caching is generally implemented in consuming applications such as web browsers,
but not software that directly accesses data, such as file transfer tools like `ftp` and `curl`.
Caching can be implemented at multiple levels, for example in database server, web server,
web proxy server, web browser, although this documentation focuses on web browser caching.

Caching is generally good.  However, there are also trade-offs.
For example, cached resources take hard-disk and memory space on the local device.
Therefore, the cache will fill up at some point and cached resources will be recycled.
Perhaps more important is to ensure that the caching mechanism does not result in
a user of information seeing old cached versions when there are new versions available.

The control of caching features in software
can be controlled on the publishing and receiving end.
For example, publishers of content can force caching to be disabled for some resources,
for example, if those resources are expected to change very frequently.
The control of caching should initially be configured by the publisher of content because
they know more about the content.
Default caching settings in web browsers may or may not be appropriate
but in general asking users to change these things is probably asking for trouble.

The following sections present several common case studies that illustrate caching issues.
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

## Case Study - Static Website Local Development Environment ##

Development of web applications typically involves using a local development environment
"sand box" where the application runs using a limited environment.
The more complex the software, the more complex the development environment needs to be.

A simple static website application (no server application other than web server for website files)
may involve only HTML, CSS, JavaScript, and data files, such as
comma-separated-value data and GeoJSON files visualization.
The web application may access data resources in several ways:

* Use the URL for a data file directly, such as a relative path URL with files found in the website file tree.
* Pass the URL of a data file to a JavaScript library, such as Leaflet web mapping URL for a map layer.
* Use the URL of remote resource, such as including in an
[HTML `<iframe>`](https://www.w3schools.com/tags/tag_iframe.asp).
* Use Ajax approach to read a data file or web service output from the same or different server.

In all cases, the web developer must be able to effectively test the application during development
with frequent changes to source files and data.
Confusion about which version of code and data are used can waste time during development.

The development environment typically includes a way to run a local web server to serve the application files.
If the application uses only core technologies (HTML, CSS, JavaScript), then it is convenient to
serve the application using a simple web server, such as Python
(see: [Big list of http static server one-liners](https://gist.github.com/willurd/5720255)).

How then can resources and technologies be configured to provide the most efficient development environment?
Need to document recommendations.

## Case Study - Deployed Static Website ##

A static website application that is developed similar to the previous case study can be deployed to the web
with little or no modification.
For example, copy the files to an Amazon S3 bucket location that is configured as a public static website.

How can the following be guaranteed?

* Updated static website files where content has changed should result in
web browser detecting and re-download the file.
This should be the case for top-level files such as HTML, as well as referenced data, JavaScript, images, etc.
* Don't force users to do a refresh, clear cache, or run incognito.
* Minimize re-download of static resources that have not changed.
* Minimize download bandwidth hit on Amazon S3 so as to minimize hosting charges.

## Case Study - Cloud Services ##

Similar to the previous sections, how to deal with caching when the content is managed in a cloud service provider?
Amazon S3 is one example. Others are GitHub (or any version control), web mapping host services, etc.
How to ensure optimal caching?

Do these services configure content caching in a standard way?
How granular is configuration and how does the configuration consider browser caching and performance?

## How Does Web Browser Caching Work? ##

Need to document the fundamentals of how caching works, using general concepts and specifics
from common web browsers.

* How does the browser check whether a new resource is available?
* Does it rely only on the expiration time for the cached resource?
* Or is there a way to do a two-pass check where the first pass is
to get the header properties and if it is expired go get the full resource?

## Options for Controlling Caching in Content Files ##

Need to discuss:

* HTML `<meta>` tag.
* HTML `<file>` tag?
* Best practices for `Last-Modified` and `ETag`, etc.
* Is it necessary to change the name on JavaScript files to reflect version change.
* How to add caching configuration when the content file format does not support (CSV, GeoJSON, etc.) - is server configuration needed?
* See:  [Two Simple Rules for HTTP Caching](https://blog.httpwatch.com/2007/12/10/two-simple-rules-for-http-caching/) and similar articles.
* How are images and other files handled?

## Options for Controlling Caching in Web Services ##

* What are best practices for response headers for web services?
* How does this play with mime types?

## Options for Controlling Caching in Cloud Hosted Platforms ##

Need to discuss the following, with the goal to understand how caching is implemented and controlled
to result in best overall performance and user experience:

* What happens to resources hosted in GitHub?
* What happens to resources hosted in Amazon S3?  Publishing rules?
* etc.
