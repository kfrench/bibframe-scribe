BIBFRAME Scribe
===============

There are two pieces to this software.  The UI can be run locally without
a webserver out of the box.  Just visit the file `app/static/index.html`
in a browser.

The backend is based on Node.js and MongoDB.  In order to avoid a confused
Node.js installation, consider using a virtual environment as provided by
[nodeenv](https://github.com/ekalinin/nodeenv), which provides `npm` and `node`
per environment and can be easily installed using `pip`.  You'll also need
to install MongoDB, which can be done with `brew` on a Mac.  There will
be a Chef cookbook coming soon to cover Linux server installs.

Until then.  With `nodeenv` and `mongod` binaries available:

```
% git clone [this repo]
% nodeenv -c -r bibframe-scribe/requirements.txt [env]
% mv bibframe-scribe [env]/
% cd [env]
% . bin/activate
% cd bibframe-scribe
% mongod -rest &
% cd app
% node index.js
```

There is a bug in the restify library.  You will need to either download
[the file](https://github.com/JasonGhent/node-restify/blob/bd3747da7db82507daaf9bc9d6110407063ae462/lib/plugins/static.js) as described in
[the pull request](https://github.com/mcavage/node-restify/pull/451) and modify
or replace `node_modules/restify/lib/plugins/static.js` until
the patch makes it into a release.

You can also use `static.js` provided as a sibling file for deployment.

Load Data
=========

You can pre-load RDF into the store.  Unfortunately, you can't use RDF/XML
yet.  Turtle/N3 or JSON-LD are your options.  You might use `rapper` from the
Raptor library to manipulate your existing serialization into something
this software can read immediately.  It also only reads HTTP[S] from the
command line easily, so you may want to publish your data to this software's
static directory and load from the locally hosted URL:

```
% cp graph.n3 [env]/bibframe-scribe/app/static/
% cd [env]/bibframe-scribe/app/
% node index.js &
% rdfstorejs load http://localhost:8888/static/graph.n3 --store-engine mongodb --store-mongo-domain localhost --store-name bfstore
```

Use `--store-overwrite true` if you want to obliterate your existing store
and write the contents of graph.n3 into it instead.

License
=======

This project is licensed under the Apache 2.0 license.  See the file LICENSE in this directory.

More
====

Scribe developement has been supported in part by the Library of Congress, BIBFLOW (an IMLS project of the UC Davis library) and Zepheira

* [Bibframe](http://bibframe.org/)
* [Zepheira](http://zepheira.com/)
* [Library of Congress](http://loc.gov/)
* [BIBFLOW](http://www.lib.ucdavis.edu/bibflow/)

