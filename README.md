# gobgp-node

[![Code Climate](https://codeclimate.com/github/codeout/gobgp-node.png)](https://codeclimate.com/github/codeout/gobgp-node)

gobgp library - NodeJS client for gobgpd

This is a young project which allows you to manage gobgpd remotely.

* Features
  * RIB management which is equivalent to ```gobgp global rib``` in gobgp CLI


## Requirements

gobgp-node is tested on Debian Linux and OSX.

* [Go](https://golang.org/doc/install)
  * Requires an environment variable ```$GOPATH``` to be configured
* [grpc](https://github.com/grpc/grpc/blob/master/INSTALL.md)
* [gobgp](https://github.com/osrg/gobgp)
  * v1.7 or later

## Installation

```zsh
npm install gobgp
```

This installation process builds the C-shared library from already installed gobgp in your system and links gobgp-node binary with it.

## Usage

Originate a route with gobgpd:

```js
var Gobgp = require('gobgp');
var gobgp = new Gobgp('<gobgpd address>:50051');

gobgp.addPath({family: 'ipv4-unicast'}, '10.0.0.0/24');
```

Withdraw a route from gobgpd:

```js
var Gobgp = require('gobgp');
var gobgp = new Gobgp('<gobgpd address>:50051');

gobgp.deletePath({family: 'ipv4-unicast'}, '10.0.0.0/24');
```

Show routes in gobgpd:

```js
var Gobgp = require('gobgp');
var gobgp = new Gobgp('<gobgpd address>:50051');

gobgp.getRib({family: 'ipv4-unicast'}, function(err, table) {
  console.log(table);
});
```

Originate a flowspec route with gobgpd:

```js
var Gobgp = require('gobgp');
var gobgp = new Gobgp('<gobgpd address>:50051');

gobgp.addPath({family: 'ipv4-flowspec'}, 'match source 10.0.0.0/24 then rate-limit 10000');
```

Show flowspec routes in gobgpd:

```js
var Gobgp = require('gobgp');
var gobgp = new Gobgp('<gobgpd address>:50051');

gobgp.getRib({family: 'ipv4-flowspec'}, function(err, table) {
  console.log(table);
});
```

Originate a BGP community added route:

```js
var Gobgp = require('gobgp');
var gobgp = new Gobgp('<gobgpd address>:50051');
var path = gobgp.serializePath('ipv4-unicast', '10.0.0.0/24');

path.pattrs.push(new Buffer([
  0xc0,                      // Optional, Transitive
  0x08,                      // Type Code: Communities
  0x04,                      // Length
  0xff, 0xff, 0xff, 0x02]))  // NO_ADVERTISE

gobgp.addPath({family: 'ipv4-unicast'}, path);
```

Or extremely easy on the latest version of gobgp,

```js
var Gobgp = require('gobgp');
var gobgp = new Gobgp('<gobgpd address>:50051');

gobgp.addPath({family: 'ipv4-unicast'}, '10.0.0.0/24 community no-advertise');
```

## Copyright and License

Copyright (c) 2016 Shintaro Kojima. Code released under the [MIT license](LICENSE).
