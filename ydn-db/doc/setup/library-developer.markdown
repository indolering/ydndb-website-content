---
layout: ydndb-article
title: "For library developer"
article:
  written_on: 2014-05-18
  updated_on: 2014-05-18
  order: 7
collection: ydndb-setup
authors:
  - kyawtun

---

{% wrap content %}


If you haven't try [Closure Tools](https://developers.google.com/closure/) before,
setup can be time consuming and painful. I recommend to read
Michael Bolin book's [Closure: The Definitive Guide](http://shop.oreilly.com/product/0636920001416.do).
A good understanding of closure coding pattern is necessary to understand and
follow this library codes.

[Apache ant](http://ant.apache.org/) is used to build javascript compiler. ydn-base repo
[build.xml](https://bitbucket.org/ytkyaw/ydn-base/raw/master/build.xml) defines compiler
and others tools setting. You must change according to your local machine setting.
Specifically check property values of `closure-library.dir` and `closure-compiler.dir`, which
point to respective directries.

Downloads the following three repos a directory.

    svn checkout http://closure-library.googlecode.com/svn/trunk/
    git clone git@bitbucket.org:ytkyaw/ydn-db.git
    git clone https://bitbucket.org/ytkyaw/ydn-base.git

that should create three directories for closure-library, ydn-base and ydn-db.

Run local apache (recommended) or a static server on that directory.

Go to ydn-db folder and run `ant deps` and `ant ydn-base.deps` to generate closure dependency tree.

Use HTML files in the /test folder for getting started. These files are also
used for debug development.

Note: we use master track version of closure tools. Compiling with pre-build jar
may encounter compile error.

Note: precompile files are built by using custom compiler to strip debug messages.
See detail on ydn-base/tools/strip_debug.txt.

Additional features requires the following optional repos.

1. Full text search https://github.com/yathit/ydn-db-fulltext.git
2. Dependency for ydn-db-fulltext https://github.com/yathit/fullproof
3. Dependency for ydn-db-fulltext https://github.com/yathit/natural
4. Synchronization https://bitbucket.org/ytkyaw/ydn-db-sync (private)


## Testing ##

You should able to run /ydn-db/test/all-test.html or run individually.
Since all test are async, disable run inparallel check box.
These test files are for basic testing and debugging.

Coverage test is performed by [JsTestDriver](http://code.google.com/p/js-test-driver/)
test. Notice that `ant gen-alltest-js` generate jsTestDriver.conf to prepare testing
configuration.

    java -jar JsTestDriver.jar --tests all

End-to-end testing for disteribution can be found in test/qunit folder as well
 as online [qunit test kits] (http://dev.yathit.com/index/demos.html).


## Contributing ##

Sending pull request is easiest way. For more, email to one of the authors in
the source code.

We follow [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml).
All commit on master branch must pass most stringent setting compilation and all unit tests.

Few coding dialect we have as follow:

* Preferred variable naming is `like_this` `notLikeThis`. For function name, `useLikeThis` as usual.
* Assume native types (boolean, number, string) are not nullable. If nullable type is used,
it is different from `undefined`. Using `undefined` for missing value in native type
is encourage over `null`.


## Library design ##

* Library API should be similar to IndexedDB API and use exact
terminology and concept in the IndexedDB specification. So that, people
 who already familiar with it can pick up immediately as well as go forward
 with native API.
* Simple operations should be easy to use as well as optimized for it.
Also impose user to use efficient
methods while making inefficient ways very difficult or impossible.
* For complex query, helper utility functions and classes will be provided.
Storage class has deep understanding about these helper classes and do
optimization behind the sense.
* Memory efficient and must not use buffer memory. If buffer is used, it must
 be explicit. Memory leak is unacceptable.
* Provide extensive error and log message in debug mode, spare no expense since
 we will strip them in production binary. Error and exception should be
thrown as soon as possible, preferable before async callback.
* Since this API is very simple, fallback to WebSQL and WebStorage should
be straight forward. This library design have no consideration for these
storage mechanisms.


## Bug report ##

Please [file an issue](https://bitbucket.org/ytkyaw/ydn-db/issues/new) for bug
report describing how we could reproduce the problem. Any subtle problem,
memory/speed performance issue and missing feature from stand point of IndexedDB
API will be considered.

You may also ask question in [Stackoverflow #ydn-db](http://stackoverflow.com/questions/tagged/ydn-db)
with ydb-db hash, or follow on Twitter [@yathit](https://twitter.com/yathit).


## License ##
Licensed under the Apache License, Version 2.0

{% endwrap %} 