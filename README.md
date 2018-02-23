# markdown-it-table-of-contents
A table of contents plugin for Markdown-it. Based on https://github.com/oktavilla/markdown-it-table-of-contents but
with some additional features to support ILM attribute based overrides. For example, in ILM, only headers with an explicit class .toc(1-4) get added to the table of contents at the level specified. Moreover the optional attribute 'toc' overrides the contents displayed. 

(ILM is a private text editor for producing custom audio-aligned ebooks supporting random-access to contents.)

For example:

```
# Title not Shown TOC {.title}

## Chapter 4. This is Shown in TOC but with a much shorter display title   {toc="4. Shorter Title"}

### This is a subtitle not shown in TOC {.notoc}

### This one is shown in TOC
```

 
## Usage

``` javascript
var MarkdownIt = require("markdown-it");
var md = new MarkdownIt();

md.use(require("markdown-it-anchor")); // Optional, but makes sense as you really want to link to something
md.use(require("markdown-it-toc-ilm"));
```

Then add `[[toc]]` where you want the table of contents to be added in your markdown.

When using with md-ilm module, the parsing object returns a field `toc` with a useful Table of Contents object.

## Options

You may specify options when `use`ing the plugin. like so:
``` javascript
md.use(require("markdown-it-table-of-contents"), options);
```

These options are available:

Name              | Description                                         | Default
------------------|-----------------------------------------------------|------------------------------------
"includeLevel"    | Headings levels to use (2 for h2:s etc)             | [1, 2]
"containerClass"  | The class for the container DIV                     | "table-of-contents"
"slugify"         | A custom slugification function                     | [string.js' `slugify`][slugify]
"markerPattern"   | Regex pattern of the marker to be replaced with TOC | `/^\[\[toc\]\]/im`
"listType"        | Type of list (`ul` for unordered, `ol` for ordered) | `ul`
"format"          | A function for formatting headings (see below)      | `undefined`
"ilmStyle"        | Follow ilm-style TOC rules (keep compatibility)     |  false
"override_toc"    | A function for overriding default selection rules   | `undefined`


`format` is an optional function for changing how the headings are displayed in the TOC.
```js
function format(headingAsString) {
  // manipulate the headings as you like here.
  return manipulatedHeadingString;
}
```

`override_toc` is an optional function for overriding header selection rules.
```js
function override_toc(heading, op) {
  // op = {isShown (true), displayText (heading), level (1-4}
  // read heading.text, heading.classes, heading.attrs, heading.tag
  // modify and return op: {isShown (bool), displayText (str), level (1-4)} 
  return op;
}
```

[slugify]: http://stringjs.com/#methods/slugify
