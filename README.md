# Hugo + NetlifyCMS = Hugolifycms

This tutorial demostrates how to integrate Hugo site and Netlify CMS, with N levels (nested sections/folders). It will be easily exportable to other [SSG](https://www.staticgen.com/))

Repo: https://github.com/davidayalas/static-site-uoc/

## Features

* Hugo with nested folders integrated with NetlifyCMS
* Multilanguage (Netlifycms and Hugo suport for multilanguage are incompatibles by now) 
* CMS accessible from live site

## Motivation

* Hugo is great to manage static sites
* Netlify CMS is an amazing and easy to use CMS in front of static site generators (and awesome if you deploy your web to Netlify)
* If your Hugo site has a structure based on nested folders, is OK from Hugo side but difficult from Netlify CMS. 
* This strategy will help you to manage both, and from your live site, access to NetlifyCMS administration only for the current section you are visiting (content folder from Hugo). This simplifies the lookup for content, because you access it from your live site and then you edit it.

## Setup

Fork the project from github interface https://github.com/davidayalas/static-site-uoc

or

		$ git clone https://github.com/davidayalas/static-site-uoc
		$ ... SETUP YOUR OWN REPO

(OPTIONAL) if you want to build the repo locally, you need to execute from your repo root

		$ node tasks/rename-languages.js 
		$ hugo


When you publish to your repo, rename content files to {{filename}}-{{language}}.md

		$ node ./tasks/rename-languages.js -

Then

		$ git add -A
		$ git commit -m "new version"		
		$ git push

(change the url from the button with you own repo)

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/davidayalas/static-site-uoc)

## Netlify setup

1. Build command:

		$ node ./tasks/rename-languages.js && node ./tasks/create-relative-cms.js && hugo

	![deploy settings](img/deploy-settings.png)

2. Setup identity

3. Setup git gateway

## Key files to setup this strategy

* Nodejs files to help the build process:

	- Rename languages: [rename-languages.js](https://github.com/davidayalas/static-site-uoc/blob/master/tasks/rename-languages.js)
		- it changes between Hugo and Netlify language management (in fact, Netlify doesn't manages languages in filenames)
		- when you build to store in git, you need to set filenames to {{filename}}-{{language}}.md
		- when you build to generate HTML, you need to set filenames to {{filename}}.{{language}}.md

	- Create relative CMS to content/section: [create-relative-cms.js](https://github.com/davidayalas/static-site-uoc/blob/master/tasks/create-relative-cms.js)
		- it loops over ./content folder and creates relative "admin cms" from https://github.com/davidayalas/static-site-uoc/tree/master/tasks/cms, replacing {{folder}} in [config.yml](https://github.com/davidayalas/static-site-uoc/blob/master/tasks/cms/config.yml)
		- when you build to store in git, you need to set filenames to {{filename}}-{{language}}.md
		- when you build to generate HTML, you need to set filenames to {{filename}}.{{language}}.md

* CMS in the footer of the live site:

	* Hugo partial template 
		* [cms.html](https://github.com/davidayalas/static-site-uoc/blob/master/themes/web-uoc-1/layouts/partials/cms.html)
		* it adds links to login (netlify identity), create new sections, new pages, edit pages, ...
		* this template is only visible if param cms=true is attached to the url.

	* Static file [cms.js](https://github.com/davidayalas/static-site-uoc/blob/master/themes/web-uoc-1/static/js/cms.js) to manage visibility and "create section" directly to git

		* It push a version of [static/admin/_index.md](https://github.com/davidayalas/static-site-uoc/blob/master/static/admin/_index.md) to git for every of your configured languages. You can setup your frontmatter accordingly to your content type in config.yml

* There are two "instances" of Netlify CMS:
	* Usual [static/admin](https://github.com/davidayalas/static-site-uoc/tree/master/static/admin), recommended to manage home staff
	* [tasks/cms](https://github.com/davidayalas/static-site-uoc/tree/master/tasks/cms) to manage the content related with a section

## How it works

* Live demo: https://site-uoc.netlify.com/
* Live demo with CMS (see the footer): https://site-uoc.netlify.com/en/?cms=true




