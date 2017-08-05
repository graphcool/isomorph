# prep [![npm version](https://badge.fury.io/js/prep.svg)](https://badge.fury.io/js/prep) [![CircleCI](https://circleci.com/gh/graphcool/prep.svg?style=svg)](https://circleci.com/gh/graphcool/prep)

Pre-renders your web app into static HTML based on your specified routes enabling SEO for single page applications.

> NOTE: `prep` is now based on [Chromeless](https://github.com/graphcool/chromeless). We'll shortly release an updated version.

## Features

* 🔎 Makes your single page app SEO friendly
* 🚀 Improves loading speed up to 400x
* ✨ Incredibly flexible and easy to use 
* 📦 Works out-of-the-box with any framework (React, Angular, Backbone...). No code-changes needed.

## Install

```sh
npm install -g prep
```

## Usage

Just run `prep` in your terminal or add it to the `scripts` as part of your build step in your `package.json`. If you don't provide a `target-dir` the contents of the `source-dir` will be overwritten.

```sh
  Usage: prep [options] <source-dir> [target-dir]

  Options:

    -h, --help           output usage information
    -c, --config [path]  Config file (Default: prep.js)
    -p, --port [port]    Phantom server port (Default: 45678)
```

In order to configure the routes which you'd like to pre-render you need to specifiy them in a Javascript config file with the following schema. If you don't provide a config file, `prep` will just pre-render the `/` route.

```js
const defaultConfig = {
  routes: ['/'],
  timeout: 1000,
  dimensions: {
    width: 1440,
    height: 900,
  },
  https: false,
  hostname: 'http://localhost',
  useragent: 'Prep',
  minify: false,
  concurrency: 4,
  additionalSitemapUrls: [],
}
```

* `routes` specifies the list of routes that the renderer should pass. (Default: `['/']`)
* `timeout` is the timeout for how long the renderer should wait for network requests. (Default: `1000`)
* `dimensions` the page dimensions in pixels that the renderer should use to render the site. (Default: `1440` x `900`)
* `https` prep uses https if true otherwise http
* `hostname` is used to show the correct urls in the generated sitemap that is stored in `[target-dir]/sitemap.xml`
* `useragent` is set a `navigator.userAgent`
* `minify` is a boolean or a [html-minifier](https://github.com/kangax/html-minifier) configuration object.
* `concurrency` controls how many routes are pre-rendered in parallel. (Default: `4`)
* `additionalSitemapUrls` is a list of URLs to include as well to the generated `sitemap.xml`. (Default: `[]`)

## Example `prep.js`

There are three different ways to configure `prep`. Which one you pick depends on your use case.

### 1. Javascript Object

The probably easiest way is to export a simple Javascript object.

```js
exports.default = {
  routes: [
    '/',
    '/world'
  ]
}
```

### 2. Synchronous Function

You can also return a function that returns the config for `prep`.

```js
exports.default = () => {
  return {
    routes: [
      '/',
      '/world'
    ]
  }
}
```

### 3. Asynchronous Function (Promise)

Furthermore you can also return a `Promise` or use ES7 features such as `async` & `await` (Babel pre-compile step needed).

```js
export default async () => {
  const routes = await getRoutesAsync()
  return { routes }
}
```

## How it works

The concept behind `prep` is very simple. `prep` starts a temporary local webserver and opens your provided routes via [PhantomJS](http://phantomjs.org/). Each route will be exported as a static HTML file. The resulting folder structure is the same as the structure of your routes.

## Known Issues

 - If you want to use `Object.assign()` in your code, please add a polyfill like [phantomjs-polyfill-object-assign](https://github.com/chuckplantain/phantomjs-polyfill-object-assign), because prep uses PhantomJS, which doesn't support `Object.assign()` yet.


## Help & Community [![Slack Status](https://slack.graph.cool/badge.svg)](https://slack.graph.cool)

Join our [Slack community](http://slack.graph.cool/) if you run into issues or have questions. We love talking to you!

![](http://i.imgur.com/5RHR6Ku.png)
