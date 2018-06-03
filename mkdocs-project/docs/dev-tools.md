# Learn Web Development / Development Tools

This page provides information about web development tools, which can commonly be used.

* [Introduction](#introduction)
* [Operating System](#operating-system)
* [Integrated Development Environment](#integrated-development-environment)
* [Text Editor](#text-editor)
* [Web Browser](#web-browser)
* [Web Browser Plugins and Add-ons](#web-browser-plugins-and-add-ons)
* [Web Server](#web-server)
* [Version Control](#version-control)
* [Test Framework](#test-framework)

----

## Introduction ##

Web software development requires that suitable development environment and tools are used.
This documentation focuses mainly on client-side software applications,
although some content also applies to server-side software.

## Operating System ##

A suitable operating system should be chosen for web development,
compatible with the software developer's skills,
and the requirements of the software.
Developing on the operating system that is most likely to be used by software users
will generally uncover the most issues with the software during development and
result in higher quality software.

It may be appropriate to use multiple operating systems during development,
for example use Microsoft Windows and Cygwin in conjunction,
in order to increase productivity.

Multiple developers on a software product may also choose to each use different
operating systems and development environments.
This may result in higher-quality software because multiple perspectives are used
(as long as such an approach does not result in lower efficiency and confusion by developers).

The choice of operating system will to some degree impact the choices made for development tools.
For greatest flexibility it is best to choose technologies
that are not tied to a specific operating system.
This is one reason whey web applications are popular
(because they run in web browsers on any operating system).

## Integrated Development Environment ##

An Integrated Development Environment (IDE) may or may not be used for web development.
If not using an IDE, then the choice of [text editor](#text-editor) will be important.

A IDE is often useful if developing server-side software because it can manage production
web server, database, version control, and other resources.

This documentation does not currently focus on IDE selection given that a suitable text editor
can often provide features that support productive web development.

## Text Editor ##

The productivity of a software developer is greatly impacted by the text editor used to edit code and other content.
Developers typically select an editor based on recommendations and personal experience,
switching when a tool does not perform as needed.
The following are text editors that are commonly used at the Open Water Foundation, listed alphabetically.

* [Atom](https://atom.io/) - developed by Google, includes Markdown editor/viewer
* [Sublime Text](https://www.sublimetext.com/) - full-features editor
* [vim, vi](https://en.wikipedia.org/wiki/Vim_(text_editor)) - useful in text windows such as Cygwin, Git Bash, and Linux shells

## Web Browser ##

Client-side web applications run in a web browser and therefore one or more suitable web browsers
must be chosen to test the application.
Modern web browsers often include developer tools to facilitate development.
Software developers should choose a browser that provides needed features,
if necessary also implementing [plugins and add-on software](#web-browser-plugins-and-add-ons).
The following are common web browsers that have built-in development tools:

* [Chrome](https://www.google.com/chrome/)
* [Firefox](https://www.mozilla.org/en-US/firefox/new/)
* [Microsoft Edge](https://www.microsoft.com/en-us/windows/microsoft-edge)

Multiple web browsers should also be used to test web applications, to ensure that the software
functions similarly on multiple browsers.  See also the [Test Framework](#test-framework) section.

## Web Browser Plugins and Add-ons##

Web browser plugins and add-on applications may also increase the efficiency of web development.
The following are useful plugins and add-ons:

* [Postman](https://www.getpostman.com/) - troubleshoot REST APIs

## Web Server ##

Server-side web applications are typically tested in an IDE that run an integrated web server.

Browser-side web applications developed using an IDE may also use an integrated IDE.
Another approach is to run a simple web server.  See the following resources:

* [Big list of http static server one-lines](https://gist.github.com/willurd/5720255) - serve static website content

## Version Control ##

A suitable version control system should be used when developing web applications.
The Open Water Foundation typically uses Git/GitHub.
See the [OWF GitHub pages](https://github.OpenWaterFoundation.org) for examples of web applications maintained on GitHub.

## Test Framework ##

A suitable test framework should be used with web applications to automate software testing.
This section will be expanded in the future.
