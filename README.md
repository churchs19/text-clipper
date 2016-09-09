text-clipper.js
===============

Fast and correct clip functions for HTML and plain text.

[![Build Status](https://travis-ci.org/arendjr/text-clipper.svg?branch=master)](https://travis-ci.org/arendjr/text-clipper)
[![text-clipper on NPM](https://img.shields.io/npm/v/text-clipper.svg)](https://www.npmjs.com/package/text-clipper)

Written by [Arend van Beelen jr.](https://github.com/arendjr) @ [Speakap](https://www.speakap.com).

Why use text-clipper?
---------------------

text-clipper offers the following advantages over similar libraries that allow clipping HTML:

 * **Correctness**
   * HTML is processed through a proper state machine, no regular expression hacks.
   * Valid HTML input always produces valid HTML output.
   * Heavily unit-tested to support the above statement.
 * **Proper Unicode handling**
   * Unicode-awareness makes sure Unicode characters such as emojis don't get clipped halfway.
 * **Performance**
   * Text-clipper has been carefully optimized and is typically as fast as or faster than its
     competitors (*).
 * **Consistent API and behavior for both HTML and plain text**

*) Performance metrics:

With text with a few HTML tags:

    text-clipper x 426,806 ops/sec ±4.33% (48 runs sampled)
    html-truncate x 146,281 ops/sec ±3.40% (53 runs sampled)
    trim-html x 55,098 ops/sec ±3.05% (50 runs sampled)
    truncate-html x 1,503 ops/sec ±3.62% (48 runs sampled)
    Fastest is text-clipper

With text with a lot of HTML tags:

    text-clipper x 86,188 ops/sec ±3.29% (50 runs sampled)
    html-truncate x 81,159 ops/sec ±4.08% (48 runs sampled)
    trim-html x 42,780 ops/sec ±2.43% (53 runs sampled)
    truncate-html x 1,273 ops/sec ±5.09% (54 runs sampled)
    Fastest is text-clipper

Setup
-----

Make sure you have Node.js installed and run:

    $ npm install text-clipper

Usage
-----

Basic example:

    var clip = require('text-clipper');
    var clippedString = clip(string, 80); // returns a string of at most 80 characters

You can also specify options:

    var clippedHtml = clip(htmlString, 140, { html: true, maxLines: 5 });

Options
-------

### breakWords

By default, text-clipper tries to break only at word boundaries so words don't get clipped halfway.
Set this option to `true` if you want words to be broken up.

### html

By default, text-clipper treats the input string as plain text. This is undesirable if the input
string is HTML, because it might result in broken HTML tags. Set this option to `true` to make
text-clipper treat the input as HTML, in which case it will try to always return valid HTML,
provided the input is valid as well.

### imageWeight

The amount of characters to assume for images. This is used whenever an image is encountered, but
also for embedded SVG and MathML content. The default is 2.

### indicator

The string to insert to indicate the string was clipped. Default: `'…'`.

Note the indicator is only inserted when the clipping doesn't occur at a line-break.

### maxLines

Maximum amount of lines allowed. If given, the string will be clipped either at the moment the
maximum amount of characters is exceeded or the moment maxLines newlines are discovered, whichever
comes first.

Note when in HTML mode, text-clipper assumes the text will be displayed with a CSS white-space
setting that treats `\n` as a line break. Of course the HTML tag `<br>` is also counted.
