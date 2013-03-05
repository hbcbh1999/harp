# Harp
----------------

Harp is an open source Asset Pipeline Framework (aka Static Site Generator) for rapidly building responsive HTML5 applications. What makes Harp unique is that it has a Platform built from the ground up for hosting Harp applications (through Dropbox no less). See [harp.io](http://harp.io) for more details.

What is an Asset Pipeline Framework you ask? An Asset Pipeline Framework offers the best tradeoffs between Static Site Generator (such as Jekyll) and a Full Stack Framework such as (Ruby on Rails). It compiles to static assets but has some runtime functionality that SSGs typically don't have (such as redirects and auth). The asset pipeline is fist-class requiring no configuration to get started.

## Table of Contents

- [Features](#features)
- [Installation](#installation)
- [The Rules (how it works)](#rules)
  - [Rule 1 - Convention over configuration](#rules-1)
  - [Rule 2 - Public directory is public](#rules-2)
  - [Rule 3 - Ignore those which start with underscore](#rules-3)
  - [Rule 4 - Dead simple asset pipeline](#rules-4)
  - [Rule 5 - Flexible metadata](#rules-5)
- [Docs](#docs)
  - [CLI Usage](#cli-usage)
  - [Lib Usage](#lib-usage)
- [Contributing](#contributing)
- [License](#license)

<a name="features"/>
### Features

  - **asset pipeline** - built-in asset pipeline for seamlessly serving of [jade](http://jade-lang.com/) and [less](http://lesscss.org/) files.
  - **global variables** - specify global variables to be available in all your templates.
  - **selected state** - a `current` object is available in all your templates for determining the current page.
  - **traverse filesystem** - iterate over your filesystem to easily generate things like an html5 cache manifest file.
  - **asset ordering** - easy to list the order your assets are referenced.
  - **server** - harp ships with a built-in server (great for development).

Maintained by [@sintaxi](http://twitter.com/HarpPlatform). Made for the [@HarpPlatform](http://twitter.com/HarpPlatform).

<a name="installation"/>
### Installation

    npm install -g harp

<a name="rules"/>
----------------
# The Rules
----------------

Rather than offering a complex feature set, harp has simple rules on how it works. Harp is a katana, not a swiss army knife. By understanding the rules, one will know how to effectively use harp.

<a name="rules-1"/>
### 1) Convention over Configuration.

**Explanation:** Harp will function with as little as a `public/index.html` file and doesn't require any configuration to get going. To add more routes just add more files. All harp's features are based off conventions that you will discover by learning the rest of these rules.

**Design Rationale:** By using convention over configuration, harp is easier to learn. If you want a tool that is highly configurable there are many out there (such as grunt). Harp gives you a sane development.

**Diagram:**

    myapp.harp.io/                    <-- root of your application (assets in the root not served)
      |- harp.json                    <-- configuration, globals goes here.
      +- public/                      <-- your application assets belong in the public dir
          |- _layout.jade             <-- optional layout file
          |- index.jade               <-- must have an index.html or index.jade file
          |- _shared/                 <-- arbitrary directory for shared partials
          |   +- nav.jade             <-- a partial for navigation
          +- aritcles/                <-- pages in here will have "/articles/" in URL (old school style)
              |- _data.json           <-- articles metadata goes here
              +- hello-world.jade     <-- must have an index.html or index.jade file

<a name="rules-2"/>
### Rule 2) Public Directory is public.

Your `public` directory defines what will be served and what URL your application exposes. Public assets belong in the `public` directory and assets outside of the `public` directory will be ignored.

    myapp.harp.io/
      |- README.md                    <--- won't be served
      |- secrets.txt                  <--- won't be served
      +- public/                      <--- required public directory
          +- index.html               <--- will be served

<a name="rules-3"/>
### Rule 3) Ignore those which start with underscore.

**Explanation:** Any files or directories that begin with underscore will be ignored by the server which makes it the recommended naming convention for `layout` and `partial` files. 

**Design Rationale:** TBD

**Diagram:**

    myapp.harp.io/
      +- public/
          |- index.html               <--- will be served
          |- _some-partial.jade       <--- won't be served
          +- _shared-partials/        <--- won't be served
              +- nav.jade

<a name="rules-4"/>
### Rule 4) Dead simple asset pipeline.

Both `jade` and `less` are built into harp. Just add an extension of `.jade` or `.less` to your file and harp's asset pipeline will do the rest.

Harp knows how to handle jade and less files as html and css respectively. Just add the file, and reference its counterpart.
    
    myfile.jade             ->        myfile.html
    myfile.less             ->        myfile.css

If you like, you may specify which mime type the file will be served with by prefixing the extension with the desired extension.

    myfile.jade             ->        myfile.html
    myfile.xml.jade         ->        myfile.xml

...but this is optional as every extension has a default output extension. The following is the same as above...

    myfile.less             ->        myfile.css
    myfile.css.less         ->        myfile.css

<a name="rules-5"/>
### Rule 5) Flexible metadata

You Files named `_data.json` make data available to templates.

<a name="documentation"/>
# Documentation
--------------------

Harp can be used as a library or as a command line utility.

<a name="cli-usage"/>
## CLI Usage

    Usage: harp [app-path] [options]

    Options:
    
        -s, --server [port]           start a server for harp app (dynamically generates assets)
        -c, --compile [output-dir]    compiles static assets. (relative to project-path)
        -d, --dirmode [port]          host a directory of harp apps (available at http://harp.nu)

Start the server in root of your application by running...

    harp -s

You may optionally supply a port to listen on...

    harp -s 8002

Compile an application from the root of your application by running...

    harp -c

You may optionally pass in a path to where you want the compiled assets to go...

    harp -c /path/to/phonegap/project/www

<a name="lib-usage"/>
## Lib Usage

You may also use harp as a node library for compiling or running as a server.

    var harp = require("harp")

serve up harp application

    harp.server(projectPath [,args] [,callback])

compile harp application

    harp.compile(projectPath [,outputPath] [, callback])
    
<a name="contributing"/>
## Contributing

### Bug Fixes

If you find a bug you would like fixed. Open up a [ticket][https://github.com/sintaxi/harp/issues/new] with a detailed description of the bug and the expected behaviour. If you would like to fix the problem yourself please do the following steps.

1. Fork it.
2. Create a branch (`git checkout -b fix-for-that-thing`)
3. Commit a failing test (`git commit -am "adds a failing test to demonstrate that thing"`)
3. Commit a fix that makes the test pass (`git commit -am "fixes that thing"`)
4. Push to the branch (`git push origin fix-for-that-thing`)
5. Open a [Pull Request][https://github.com/sintaxi/harp/pulls]

### New Functionality

If you wish to add new functionality to harp, please provide [@sintaxi](mailto:brock@sintaxi.com) a harp application that demonstrates deficiency in current design or desired additional behaviour. Optionally, you may submit a pull request with the steps above. 

<a name="license"/>
## License

Copyright 2012 Chloi Inc. All rights reserved.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.


