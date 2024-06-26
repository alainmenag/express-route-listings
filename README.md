
List, log, or render all your express routes and their related file.

- [Install \& Import](#install--import)
- [Usage](#usage)
	- [1. Log All Routes](#1-log-all-routes)
	- [2. Live Logger](#2-live-logger)
	- [3. Web Service](#3-web-service)
	- [4. Array of Routes](#4-array-of-routes)
- [Dictionary](#dictionary)
	- [Symbols](#symbols)
	- [Log Syntax](#log-syntax)
	- [Terms](#terms)

# Install & Import

```bash
npm install express-route-listings
```

```js
const express = require('express');
const expressRouteListings = require('express-route-listings');

const app = express();
```

# Usage

## 1. Log All Routes

```js
app.listen(3001, '127.0.0.1', (e) =>
{
	// log all mounted routes
	expressRouteListings.log(app, {
		//url: 'http://localhost:8715', // (optional) to include in logging
		//output: 'json', // (optional) log route as json
		//output: null, // (default) log route as formatted text
	});
});
```

```log
🚥 2024-04-30T02:49:45.136Z [ null, '*' ] [ null, '/' ] (/routes/index.js)
🚥 2024-04-30T02:49:45.137Z [ null, '*' ] [ null, '/' ] (/routes/index.js)
🚥 2024-04-30T02:49:45.137Z [ null, '*' ] [ null, '/' ] (/routes/index.js)
🚥 2024-04-30T02:49:45.137Z [ null, '*' ] [ null, '/' ] (/routes/index.js)
🚥 2024-04-30T02:49:45.137Z [ null, '*' ] [ null, '/' ] (/routes/index.js)
🚥 2024-04-30T02:49:45.137Z [ null, '*' ] [ null, '/' ] (/routes/index.js)
🚥 2024-04-30T02:49:45.137Z [ null, 'GET' ] [ null, '/' ] (/routes/index.js)
🚥 2024-04-30T02:49:45.137Z [ null, '*' ] [ null, '/_:id' ] (/routes/api/bin.js)
🚥 2024-04-30T02:49:45.137Z [ null, '*' ] [ null, '/_:id/*' ] (/routes/api/bin.js)
🚥 2024-04-30T02:49:45.137Z [ null, '*' ] [ null, '/_*' ] (/routes/api/bin.js)
🚥 2024-04-30T02:49:45.138Z [ null, '*' ] [ null, '/api/bin/_:id' ] (/routes/api/bin.js)
🚥 2024-04-30T02:49:45.138Z [ null, '*' ] [ null, '/api/bin/_:id/*' ] (/routes/api/bin.js)
🚥 2024-04-30T02:49:45.138Z [ null, '*' ] [ null, '/api/bin/_*' ] (/routes/api/bin.js)
🚥 2024-04-30T02:49:45.138Z [ null, 'POST' ] [ null, '/api/obs/actions/restart' ] (/routes/api/obs-sub.js)
🚥 2024-04-30T02:49:45.138Z [ null, 'DELETE' ] [ null, '/api/obs/actions/group/wakeup/:pizza' ] (/routes/api/obs-sub-sub.js)
🚥 2024-04-30T02:49:45.138Z [ null, 'GET' ] [ null, '/api/obs' ] (/routes/api/obs.js)
🚥 2024-04-30T02:49:45.138Z [ null, 'GET' ] [ null, '/api/obs/:sceneUuid' ] (/routes/api/obs.js)
🚥 2024-04-30T02:49:45.138Z [ null, 'POST' ] [ null, '/api/obs/resource' ] (/routes/api/obs.js)
🚥 2024-04-30T02:49:45.138Z [ null, 'GET' ] [ null, '/api/litra' ] (/routes/api/litra.js)
🚥 2024-04-30T02:49:45.138Z [ null, 'GET' ] [ null, '/api/litra/:id' ] (/routes/api/litra.js)
🚥 2024-04-30T02:49:45.138Z [ null, 'POST' ] [ null, '/api/litra/dial/:id' ] (/routes/api/litra.js)
🚥 2024-04-30T02:49:45.138Z [ null, 'POST' ] [ null, '/api/litra/toggle/:id' ] (/routes/api/litra.js)
🚥 2024-04-30T02:49:45.138Z [ null, 'GET' ] [ null, '/api/screenshot/cleanup' ] (/routes/api/screenshot.js)
```

## 2. Live Logger
Will log each request and it's matching route details.

```js
app.use(expressRouteListings.log({
	//url: 'http://localhost:8715', // (optional) to include in logging
	//output: 'json', // (optional) log route as json
	//output: null, // (default) log route as formatted text
	//pathPrefix: 'https://website.com', // (optional) prepend url/chars to an incoming path
	//routePrefix: 'http://localhost:8715', // (optional) prepend url/chars to an incoming path
}));
```

```log
✅ 2024-04-30T02:49:48.687Z [ 'GET', 'GET' ] [ '/api/obs', '/api/obs' ] (/routes/api/obs.js)
❌ 2024-04-30T02:50:11.581Z [ 'GET', null ] [ '/api/obsd', null ] ()
✅ 2024-04-30T02:50:24.593Z [ 'GET', 'GET' ] [ '/api/litra/23424234234', '/api/litra/:id' ] (/routes/api/litra.js)
✅ 2024-04-30T02:55:00.653Z [ 'GET', 'GET' ] [ '/', '/' ] (/routes/index.js)
```

## 3. Web Service
Enable the built-in web service.

- Return your routes as an array over HTTP.

```js
app.get('/what/ever/route', expressRouteListings.web({
	//include: [ 'name', 'regexp', 'route', 'file', 'methods', 'code' ], // (default) include only certain key-value pairs
	//exclude: [ 'code' ], // (optional) do not include certain key-value pairs
}));
```

```json
[
	{
		"name": "query",
		"regexp": "/",
		"route": "/",
		"methods": [ "*" ],
		"code": "function query(req, res, next){\n...",
		"file": "/routes/index.js"
	},
	...
]
```

## 4. Array of Routes

```js
const routes = expressRouteListings.list(app, {
	exclude: [ 'code' ], // (optional) do not include certain key-value pairs
	include: [ 'name', 'regexp', 'route', 'file', 'methods', 'code' ], // (optional) include only certain key-value pairs
});

console.log(routes);
```

```js
[
	{
		name: 'query',
		regexp: '/',
		route: '/',
		methods: [ '*' ],
		code: 'function query(req, res, next){\n...'
		file: '/routes/index.js'
	},
	{
		name: 'expressInit',
		regexp: '/',
		route: '/',
		methods: [ '*' ],
		code: 'function expressInit...',
		file: '/routes/index.js'
	},

	...

	{
		name: 'bound dispatch',
		regexp: '/api/obs/resource',
		route: '/api/obs/resource',
		methods: [ 'POST' ],
		code: 'async function(req, res)...',
		file: '/routes/api/obs.js'
	}
]
```

# Dictionary

## Symbols

- 🚥 - Log existing route
- ✅ - Live log on HTTP request successfully matched to a route
- ❌ - Live log on HTTP request failed to match to a route

## Log Syntax

```
🚥 <reqTimestamp> [ '<reqMethod>', '<routeMethod>' ] [ '<reqPath>', '<routeRoute>' ] (<routeFile>)
✅ 2024-04-30T02:50:24.593Z [ 'GET', 'GET' ] [ '/api/litra/23424234234', '/api/litra/:id' ] (/routes/api/litra.js)
```

## Terms
- **reqTimestamp**: The timestamp of when the item was logged.
- **reqMethod**: The method of an incoming HTTP request.
- **routeMethod**: The route method that matched an incoming HTTP **reqMethod**.
- **reqPath**: The path of an incoming HTTP request.
- **routeRoute**: The route that matched an incoming HTTP **reqPath**.
- **routeFile**: The file that contains the matched route.
