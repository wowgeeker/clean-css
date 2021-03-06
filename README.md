[![NPM version](https://badge.fury.io/js/clean-css.png)](https://badge.fury.io/js/clean-css)
[![Build Status](https://secure.travis-ci.org/GoalSmashers/clean-css.png)](https://travis-ci.org/GoalSmashers/clean-css)
[![Dependency Status](https://gemnasium.com/GoalSmashers/clean-css.png)](https://gemnasium.com/GoalSmashers/clean-css)

## What is clean-css?

Clean-css is a [node.js](http://nodejs.org/) library for minifying CSS files.
It does the same job as YUI Compressor's CSS minifier, but much faster thanks
to many speed optimizations and node.js' V8 engine.


## Usage

### What are the requirements?

```
node.js 0.8.0+ (tested on CentOS, Ubuntu, OS X 10.6+, and Windows 7+)
```

### How to install clean-css?

```
npm install clean-css
```

### How to use clean-css CLI?

Clean-css accepts the following command line arguments (please make sure
you use `<source-file>` as the very last argument to avoid potential issues):

```
cleancss [options] <source-file>

-h, --help                  Output usage information
-v, --version               Output the version number
-e, --remove-empty          Remove empty declarations, e.g. a{}
-b, --keep-line-breaks      Keep line breaks
--s0                        Remove all special comments, i.e. /*! comment */
--s1                        Remove all special comments but the first one
-r, --root [root-path]      A root path to which resolve absolute @import rules and rebase relative URLs
-o, --output [output-file]  Use [output-file] as output instead of STDOUT
-s, --skip-import           Disable the @import processing
-d, --debug                 Shows debug information (minification time & compression efficiency)
```

#### Examples:

To minify a **public.css** file into **public-min.css** do:

```
cleancss -o public-min.css public.css
```

To minify the same **public.css** into the standard output skip the `-o` parameter:

```
cleancss public.css
```

More likely you would like to concatenate a couple of files.
If you are on a Unix-like system:

```bash
cat one.css two.css three.css | cleancss -o merged-and-minified.css
```

On Windows:

```bat
type one.css two.css three.css | cleancss -o merged-and-minified.css
```

Or even gzip the result at once:

```bash
cat one.css two.css three.css | cleancss | gzip -9 -c > merged-minified-and-gzipped.css.gz
```

### How to use clean-css programmatically?

```js
var cleanCSS = require('clean-css');
var source = 'a{font-weight:bold;}';
var minimized = cleanCSS.process(source);
```

Process method accepts a hash as a second parameter, i.e.,
`cleanCSS.process(source, options)` with the following options available:

* `keepSpecialComments` - `*` for keeping all (default), `1` for keeping first one, `0` for removing all
* `keepBreaks` - whether to keep line breaks (default is false)
* `removeEmpty` - whether to remove empty elements (default is false)
* `benchmark` - turns on benchmarking mode measuring time spent on cleaning up
  (run `npm run bench` to see example)
* `root` - path to resolve absolute `@import` rules and rebase relative URLs
* `relativeTo` - path with which to resolve relative `@import` rules and URLs
* `processImport` - whether to process `@import` rules

### What are the clean-css' dev commands?

First clone the source, then run:

* `npm run bench` for clean-css benchmarks (see [test/bench.js](https://github.com/GoalSmashers/clean-css/blob/master/test/bench.js) for details)
* `npm run check` to check JS sources with [JSHint](https://github.com/jshint/jshint/)
* `npm test` for the test suite

## Tips & Tricks

### How to preserve a comment block?

Use the `/*!` notation instead of the standard one `/*`:

```css
/*!
  Important comments included in minified output.
*/
```

### How to rebase relative image URLs

Clean-css will handle it automatically for you (since version 1.1) in the following cases:

* When using the CLI:
  1. Use an output path via `-o/--output` to rebase URLs as relative to the output file.
  2. Use a root path via `-r/--root` to rebase URLs as absolute from the given root path.
  3. If you specify both then `-r/--root` takes precendence.
* When using clean-css as a library:
  1. Use a combination of `relativeTo` and `target` options for relative rebase (same as 1 in CLI).
  2. Use a combination of `relativeTo` and `root` options for absolute rebase (same as 2 in CLI).
  3. `root` takes precendence over `target` as in CLI.

## Acknowledgments

* Vincent Voyer ([@vvo](https://github.com/vvo)) for a patch with better
  empty element regex and for inspiring us to do many performance improvements
  in 0.4 release.
* Isaac ([@facelessuser](https://github.com/facelessuser)) for pointing out
  a flaw in clean-css' stateless mode.
* Jan Michael Alonzo ([@jmalonzo](https://github.com/jmalonzo)) for a patch
  removing node.js' old `sys` package.
* [@XhmikosR](https://github.com/XhmikosR) for suggesting new features
  (option to remove special comments and strip out URLs quotation) and
  pointing out numerous improvements (JSHint, media queries).
* Anthony Barre ([@abarre](https://github.com/abarre)) for improvements to
  `@import` processing, namely introducing the `--skip-import` /
  `processImport` options.

## License

Clean-css is released under the [MIT License](https://github.com/GoalSmashers/clean-css/blob/master/LICENSE).
