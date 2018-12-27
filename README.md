# ucsc-hub-js

read and write UCSC track and assembly hub files in node or the browser

## Status

[![Build Status](https://img.shields.io/travis/com/GMOD/ucsc-hub-js/master.svg?logo=travis&style=flat-square)](https://travis-ci.com/GMOD/ucsc-hub-js)
[![Coverage Status](https://img.shields.io/codecov/c/github/GMOD/ucsc-hub-js/master.svg?logo=codecov&style=flat-square)](https://codecov.io/gh/GMOD/ucsc-hub-js/branch/master)
[![NPM version](https://img.shields.io/npm/v/@gmod/ucsc-hub.svg?logo=npm&style=flat-square)](https://npmjs.org/package/@gmod/ucsc-hub)

## API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

#### Table of Contents

-   [RaFile](#rafile)
    -   [Parameters](#parameters)
    -   [Properties](#properties)
    -   [add](#add)
        -   [Parameters](#parameters-1)
    -   [update](#update)
        -   [Parameters](#parameters-2)
    -   [delete](#delete)
        -   [Parameters](#parameters-3)
    -   [clear](#clear)
    -   [toString](#tostring)
-   [RaStanza](#rastanza)
    -   [Parameters](#parameters-4)
    -   [Properties](#properties-1)
    -   [add](#add-1)
        -   [Parameters](#parameters-5)
    -   [set](#set)
        -   [Parameters](#parameters-6)
    -   [delete](#delete-1)
        -   [Parameters](#parameters-7)
    -   [clear](#clear-1)
    -   [toString](#tostring-1)

### RaFile

**Extends Map**

Class representing an ra file. Each file is composed of multiple stanzas, and
each stanza is separated by one or more blank lines. Each stanza is stored in
a Map with the key being the value of the first key-value pair in the stanza.
The usual Map methods can be used on the file. An additional method `add()`
is available to take a raw line of text and break it up into a key and value
and add them to the class. This should be favored over `set()` when possible,
as it performs more validity checks than using `set()`.

#### Parameters

-   `raFile` **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) \| [Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>)** An ra file, either as a single
    string or an array of strings with one stanza per entry. Supports both LF
    and CRLF line terminators. (optional, default `[]`)

#### Properties

-   `nameKey` **([undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) \| [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String))** The key of the first line of all the
    stanzas (`undefined` if the stanza has no lines yet).


-   Throws **[Error](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Error)** Throws if an empty stanza is added, if the key in the first
    key-value pair of each stanze isn't the same, or if two stanzas have the same
    value for the key-value pair in their first lines.

#### add

Add a single stanza to the file

##### Parameters

-   `stanza` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** A single stanza

Returns **[RaFile](#rafile)** The RaFile object

#### update

Use `add()` if possible instead of this method. If using this, be aware
that no checks are made for comments, empty stanzas, duplicate keys, etc.

##### Parameters

-   `key` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The key of the RaFile stanza
-   `value` **[RaStanza](#rastanza)** The RaFile stanza used to replace the prior one

#### delete

Delete a stanza

##### Parameters

-   `stanza` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The name of the stanza to delete (the value in its
    first key-value pair)

Returns **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** true if the deleted stanza existed, false if it did not

#### clear

Clear all stanzas and comments

#### toString

Returns **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Returns the stanza as a string fit for writing to a ra
file. Original leading indent is preserved. It may not be the same as the
input stanza as lines that were joined with `\` in the input will be output
 as a single line and all comments will have the same indentations as the
rest of the stanza. Comments between joined lines will move before that
line.

### RaStanza

**Extends Map**

Class representing an ra file stanza. Each stanza line is split into its key
and value and stored as a Map, so the usual Map methods can be used on the
stanza. An additional method `add()` is available to take a raw line of text
and break it up into a key and value and add them to the class. This should
be favored over `set()` when possible, as it performs more validity checks
than using `set()`.

#### Parameters

-   `stanza` **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) \| [Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>)** An ra file stanza, either as a
    string or a array of strings with one line per entry. Supports both LF and
    CRLF line terminators. (optional, default `[]`)

#### Properties

-   `nameKey` **([undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) \| [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String))** The key of the first line of the
    stanza (`undefined` if the stanza has no lines yet).
-   `name` **([undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) \| [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String))** The value of the first line of the
    stanza, by which it is identified in an ra file  (`undefined` if the stanza
    has no lines yet).
-   `indent` **([undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) \| [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String))** The leading indent of the stanza,
    which is the same for every line (`undefined` if the stanza has not lines
    yet, `''` if there is no indent).


-   Throws **[Error](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Error)** Throws if the stanza has blank lines, if the first line
    doesn't have both a key and a value, if a key in the stanza is
    duplicated, or if lines in the stanza have inconsistent indentation.

#### add

Add a single line to the stanza

##### Parameters

-   `line` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** A stanza line

Returns **[RaStanza](#rastanza)** The RaStanza object

#### set

Use `add()` if possible instead of this method. If using this, be aware
that no checks are made for comments, indentation, duplicate keys, etc.

##### Parameters

-   `key` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The key of the stanza line
-   `value` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The value of the stanza line

Returns **[RaStanza](#rastanza)** The RaStanza object

#### delete

Delete a line

##### Parameters

-   `key` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The key of the line to delete

Returns **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** true if the deleted line existed, false if it did not

#### clear

Clear all lines and comments

#### toString

Returns **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Returns the stanza as a string fit for writing to a ra
file. Original leading indent is preserved. It may not be the same as the
input stanza as lines that were joined with `\` in the input will be output
as a single line and all comments will have the same indentations as the
rest of the stanza. Comments between joined lines will move before that
line.

## License

MIT © [Generic Model Organism Database Project](http://gmod.org/wiki/Main_Page)
