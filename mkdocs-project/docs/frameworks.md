# Learn Web Development / Frameworks #

* [Introduction](#introduction)
* [Learn the Core Web Technologies](#learn-the-core-web-technologies)
* [Selecting a Framework](#selecting-a-framework)

----

## Introduction

A web framework is at its core a selection of the language(s) and software tools used
to implement web technologies for the language.
Some web frameworks focus on core web technologies such as HTML, JavaScript, and CSS and
therefore the web developer mainly needs to know these technologies.
Other frameworks wrap or extend the core technologies by introducing other languages and components,
typically to allow developers to use languages with which they are more familiar,
to add more power, use server-side and/or browser features, and other reasons.

Web technology professionals and enthusiasts spend a lot of time discussing which frameworks and
languages are best.
The reality is that there is no single answer and web developers should pick technologies
and languages that match their skills and solution requirements,
as well as being suitable for the information technology environment of the target organization.
This choice may not be obvious at first and over time multiple technologies and languages may be tried.
Unfortunately, these are moving targets as software changes.
Open source technologies provide many benefits, but one of the downsides is that people are
free to develop new solutions, leading to a proliferation of options.
That is one of the reasons why there are so many jobs available in technology,
requiring a wide array of skills and experience.
Frameworks tend to adopt useful approaches from other frameworks and the choice of framework
may come down to reasons such as stability, number of users, quality of documentation, etc.

## Learn the Core Web Technologies ##

All web technologies leverage and are limited by core web technologies.
Sooner or later, a web developer will be faced with understanding HTML, CSS, JavaScript, HTTP,
CORS, caching, and other technical topics.
Therefore, it is recommended to begin gaining competence with these technologies as soon as possible,
at least to understand their use and common pitfalls.
This documentation provides references and in some cases detailed information.

Many hosted solutions, such as web mapping and website hosting (WordPress, Google Sites, etc.),
provide "drag and drop", "template", "plug-in", and "widget" user interface builders to customize the website.
However, these features can only go so far and inevitably more specific functionality is needed to
implement a custom solution.
Quite often this is implemented by using core web technologies to customize the site.
For example, learning how cascading style sheets (CSS) are used is often key to customization.

## Selecting a Framework ##

As indicated previously, selecting a framework may be impacted by developer skills,
functional requirements, and target environment.
The following are a few options (although there are many others), organized by language.
In some cases, the choice of language greatly impacts the design of the solution.
For example, using R may focus the solution towards an R Shiny hosted application.
A Drupal/PHP focuses on a server-side solution first.
The following list is a starting point and will be enhanced as time allows.
Examples from first-hand experience are given where available.

A primary consideration is how many server-side components are needed and
what will be the cost and complexity of hosting the deployed solution.
For example, can the solution live on an existing server for an organization?
Can the solution live in the cloud using backbone infrastructure such as Amazon Web Services?
It is often helpful to prototype a small solution that uses the full technology stack to
confirm that the envisioned technology solution will actually work and be cost-effective.
Technical barriers need to be resolved because they can render an overall solution unworkable.

* Web frameworks:
	+ ["Web framework" on Wikipedia](https://en.wikipedia.org/wiki/Web_framework)
	+ ["Web framework comparison" on Wikipedia](https://en.wikipedia.org/wiki/Comparison_of_web_frameworks)
* Core web technologies:
	- [HTML, W3 Schools tutorial](https://www.w3schools.com/html/)
	- [CSS, W3 Schools tutorial](https://www.w3schools.com/css/)
	- [JavaScript, W3 Schools tutorial](https://www.w3schools.com/js/)
* Java:
	+ Server-side content generation:
		- [Java Server Pages](https://en.wikipedia.org/wiki/JavaServer_Pages)
	+ Browser applications
		- [Applets](https://en.wikipedia.org/wiki/Java_applet)
* JavaScript:
	+ Server-side content generation:
		- [Node.js](https://nodejs.org/en/)
	+ Client-side content generation:
		- [Angular](https://angular.io/) - uses TypeScript
		- [Meteor](https://www.meteor.com/)
		- [React](https://reactjs.org/)
		- [Vue](https://vuejs.org/)
* PHP:
	+ Server-side content generation:
		- [Drupal](https://www.drupal.org/)
* Python:
	+ Server-side content generation:
		- [Django](https://en.wikipedia.org/wiki/Django_(web_framework))
* R:
	+ Server-side content generation:
		- [R Shiny](https://shiny.rstudio.com/)
* TypeScript (translates to JavaScript):
	+ Client-side content generation:
		- [Angular](https://angular.io/)
