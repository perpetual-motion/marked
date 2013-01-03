# marked
>> forked from https://github.com/chjj/marked

A full-featured markdown parser and compiler, written in javascript.
Built for speed.

This version incorporates the following changes:
    
    - Async support 
    - use of the pygments.appspot.com syntax highlighter
    - pluggable interface for adding your own markdown tags

**FYI**: The regular `marked` function is still available, but an async highlighter won't work with it

## Install

``` bash
$ npm install git://github.com/perpetual-motion/marked.git
```


## Options

marked has 4 different switches which change behavior.

- __pedantic__: Conform to obscure parts of `markdown.pl` as much as possible.
  Don't fix any of the original markdown bugs or poor behavior.
- __gfm__: Enable github flavored markdown (enabled by default).
- __sanitize__: Sanitize the output. Ignore any HTML that has been input.
- __highlight__: A callback to highlight code blocks.
- __blocks__: A collection of block handlers 
- __inlines__: A collection of inline handlers


## Usage

``` js
// Set default options
marked.setOptions({
  gfm: true,
  pedantic: false,
  sanitize: false,
});

marked.async('i am using __markdown__.', function(err, markedContent) {
    console.log(markedContent + '\n');
});

```

or from the command line:

`marked -o output.html src.md `

## Advanced Usage

The options for `blocks` and `inlines` let you smuggle more operations into the markdown transformation.

Both are essentially glorified search/replace options, `blocks` only operate at a block level, whereas `inlines` modify content inline.

``` javascript 
marked.setOptions({
  gfm: true,
  pedantic: false,
  sanitize: false,
  inlines: {
        // you can add one or more search/replace operations here.
        youtube: {
            search: / *\[([^\:\]]+):([^\]]+)\] *\n*/,
            replace: function (capture, tag, data) {
                if (tag == 'youtube') {
                    return '<iframe class="youtube" src="https://www.youtube.com/embed/' + data + '"></iframe>'
                }
                return 'undefined:' + tag;
            }
        }
    },
    blocks: 
});

```

## Benchmarks

node v0.4.x

``` bash
$ node test --bench
marked completed in 12071ms.
showdown (reuse converter) completed in 27387ms.
showdown (new converter) completed in 75617ms.
markdown-js completed in 70069ms.
```

node v0.6.x

``` bash
$ node test --bench
marked completed in 6448ms.
marked (gfm) completed in 7357ms.
marked (pedantic) completed in 6092ms.
discount completed in 7314ms.
showdown (reuse converter) completed in 16018ms.
showdown (new converter) completed in 18234ms.
markdown-js completed in 24270ms.
```

__Marked is now faster than Discount, which is written in C.__

For those feeling skeptical: These benchmarks run the entire markdown test suite
1000 times. The test suite tests every feature. It doesn't cater to specific
aspects.


## Another Javascript Markdown Parser

The point of marked was to create a markdown compiler where it was possible to
frequently parse huge chunks of markdown without having to worry about
caching the compiled output somehow...or blocking for an unnecesarily long time.

marked is very concise and still implements all markdown features. It is also
now fully compatible with the client-side.

marked more or less passes the official markdown test suite in its
entirety. This is important because a surprising number of markdown compilers
cannot pass more than a few tests. It was very difficult to get marked as
compliant as it is. It could have cut corners in several areas for the sake
of performance, but did not in order to be exactly what you expect in terms
of a markdown rendering. In fact, this is why marked could be considered at a
disadvantage in the benchmarks above.

Along with implementing every markdown feature, marked also implements
[GFM features](http://github.github.com/github-flavored-markdown/).

None of the above are mutually exclusive/inclusive.

## License

Copyright (c) 2011-2012, Christopher Jeffrey. (MIT License)
Changes Copyright (c) 2013, Garrett Serack. (MIT License)

See LICENSE for more info.
