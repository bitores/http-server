[![build status](https://img.shields.io/travis/indexzero/http-server.svg?style=flat-square)](https://travis-ci.org/indexzero/http-server)
[![dependencies status](https://img.shields.io/david/indexzero/http-server.svg?style=flat-square)](https://david-dm.org/indexzero/http-server)
[![npm](https://img.shields.io/npm/v/http-server.svg?style=flat-square)](https://www.npmjs.com/package/http-server)
[![license](https://img.shields.io/github/license/indexzero/http-server.svg?style=flat-square)](https://github.com/indexzero/http-server)

# http-server: a command-line http server

`http-server` is a simple, zero-configuration command-line http server.  It is powerful enough for production usage, but it's simple and hackable enough to be used for testing, local development, and learning.

![](https://github.com/nodeapps/http-server/raw/master/screenshots/public.png)

# Installing globally:

Installation via `npm`:

     npm install @huangzj/http-server -g

This will install `http-server` globally so that it may be run from the command line.

## Usage:

     http-server [path] [options]

`[path]` defaults to `./public` if the folder exists, and `./` otherwise.

*Now you can visit http://localhost:8080 to view your server*

**Note:** Caching is on by default. Add `-c-1` as an option to disable caching.

## Available Options:

`-p` Port to use (defaults to 8080)

`-a` Address to use (defaults to 0.0.0.0)

`-d` Show directory listings (defaults to `true`)

`-i` Display autoIndex (defaults to `true`)

`-g` or `--gzip` When enabled (defaults to `false`) it will serve `./public/some-file.js.gz` in place of `./public/some-file.js` when a gzipped version of the file exists and the request accepts gzip encoding.

`-e` or `--ext` Default file extension if none supplied (defaults to `html`)

`-s` or `--silent` Suppress log messages from output

`--cors` Enable CORS via the `Access-Control-Allow-Origin` header

`-o` Open browser window after starting the server

`-m` or `--mock` Add the mock data file

`-c` Set cache time (in seconds) for cache-control max-age header, e.g. `-c10` for 10 seconds (defaults to `3600`). To disable caching, use `-c-1`.

`-U` or `--utc` Use UTC time format in log messages.

`-P` or `--proxy` Proxies all requests which can't be resolved locally to the given url. e.g.: -P http://someurl.com

`-S` or `--ssl` Enable https.

`-C` or `--cert` Path to ssl cert file (default: `cert.pem`).

`-K` or `--key` Path to ssl key file (default: `key.pem`).

`-r` or `--robots` Provide a /robots.txt (whose content defaults to `User-agent: *\nDisallow: /`)

`-h` or `--help` Print this list and exit.

## Magic Files

- `index.html` will be served as the default file to any directory requests.
- `404.html` will be served if a file is not found. This can be used for Single-Page App (SPA) hosting to serve the entry page.



## mock data [new]()
- 添加 如 restfull api route listener
> #http-server -m main.js
```
var Router = require('route-emitter'),
    router = new Router();

router.listen('get', '/', function (req, res) {
  res.end('Hello, world')
})

// listen for any http verb!
router.listen('post', '/blog', function (req, res) {
  res.end('BLOG CREATED!')
})

// or you can catch 404s
// router.listen('*', '*', function (req, res) {
//   res.writeHead(404)
//   res.end('PAGE NOT FOUND!')
// })

// ...or verb-specific 404s
router.listen('put', '*', function (req, res) {
  res.writeHead(404)
  res.end('RESOURCE NOT FOUND!')
})

// create a listener with named emitter!
router.listen('delete', '/blog', 'deleteThatBlog')

// react to the emit!
router.on('deleteThatBlog', function (req, res) {
  res.end('BLOG DELETED')
})

// catch named parameters!
router.listen('get', '/blog/{{ id }}', function (req, res, params) {
  res.end(params.id)
})

// catch splats!
router.listen('delete', '/*', function (req, res, params) {
  res.end(params._splat[0]) // || res.end(params._1)
})

// or roll your own regexp!
router.listen('patch', /my\/(.*)/, function (req, res, params) {
  res.end(params._captured[0]) // || res.end(params.$1)
})

var route = router.route.bind(router);


module.exports = function(req, res){

  route(req, res)
}
```

# Development

Checkout this repository locally, then:

```sh
$ npm i
$ node bin/http-server
```

*Now you can visit http://localhost:8080 to view your server*

You should see the turtle image in the screenshot above hosted at that URL. See
the `./public` folder for demo content.


