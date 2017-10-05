Unmaintained
============

The original maintainer of this project no longer uses nano and hence
has very little motivation to continue working on it. It remains
available on GitHub for archival purposes only.

I highly recommend using [dte] as a simple but much more capable editor.

[dte] screenshot:

![dte screenshot](https://craigbarnes.gitlab.io/dte/screenshot.png)


------


Description
-----------

The syntax highlighting definitions that come bundled with nano are of
pretty poor quality. This is an attempt at providing a good set of accurate
syntax definitions to replace and expand the defaults.

Installation
------------

Using `make install` will install the syntax definitions to the
`~/.nano/syntax/` directory.

To enable highlighting for *all* languages after installation, add the
following command to your `~/.nanorc` file:

    include ~/.nano/syntax/ALL.nanorc

To enable only a subset of languages, `include` them individually:

    include ~/.nano/syntax/c.nanorc
    include ~/.nano/syntax/python.nanorc
    include ~/.nano/syntax/sh.nanorc
    # ...

If you prefer to install to a location that all users can access, using
`sudo make install-global` will install to `/usr/local/share/nano/`.
Syntax files installed under this directory can then be `include`d in
either `/etc/nanorc` or any user's personal `~/.nanorc`.

**Note**: If your terminal **text** color isn't black, you'll need to
specify it when installing, using `make install TEXT=color`, where
`color` must be one of: `red`, `green`, `yellow`, `blue`, `magenta`,
`cyan` or `white`.

After installation, the various source code samples in the `examples`
directory can be used to check that highlighting is working correctly.
If it doesn't work as expected, see the FAQ below.

Theme System
------------

All `*.nanorc` files are passed through [mixins.sed] and [theme.sed] before
installation. These scripts allow rules to be specified in terms of token
names or [mixins], instead of hard-coded colors.

For example, the following named rule:

    TYPE: "int|bool|string"

becomes:

    color green "int|bool|string"

and the following "mixin":

    +BOOLEAN

becomes:

    color brightcyan "\<(true|false)\>"

This system helps to keep colors uniform across different languages and
also to keep the definitions clear and maintainable, which is something that
becomes quite awkward using only plain [nanorc] files.

**Note:** if `~/.nanotheme` exists it will be used as a custom theme, in
place of [theme.sed]. A custom theme may also be specified by installing
with `make THEME=your-custom-theme.sed`. Themes must be valid sed scripts,
defining *all* color codes found in [theme.sed] in order to work correctly.

FAQ
----

### Why does syntax highlighting only work for a subset of supported files?

There appears to be a bug in older versions of nano that causes
highlighting to fail when `/etc/nanorc` and `~/.nanorc` both contain
`syntax` rules. The usual workaround is to remove all `syntax` and `include`
commands from one file or the other, or to use a newer version of nano.

### Why do I get weird errors when running nano < 2.1.5 on *BSD systems?

In order to reliably highlight keywords, this projects makes heavy use of
the GNU regex word boundary extensions (`\<` and `\>`). BSD implementations
also have these extensions but use a different, incompatible syntax
(`[[:<:]]` and `[[:>:]]`). Since version 2.1.5, nano can automatically
translate the GNU syntax to BSD syntax at run-time, but for the benefit of
people running a pre-2.1.5 version of nano on OS X or *BSD, the `.nanorc`
file itself can be translated by installing with `make BSDREGEX=1`.

### Why not use `\s` instead of the verbose `[[:space:]]` pattern?

Because nano compiles against the platform's native regex library and some
platforms don't support `\s` (as it's not required by POSIX [ERE]).

Unlicense
---------

This is free and unencumbered software released into the public domain.

Anyone is free to copy, modify, publish, use, compile, sell, or
distribute this software, either in source code form or as a compiled
binary, for any purpose, commercial or non-commercial, and by any
means.

In jurisdictions that recognize copyright laws, the author or authors
of this software dedicate any and all copyright interest in the
software to the public domain. We make this dedication for the benefit
of the public at large and to the detriment of our heirs and
successors. We intend this dedication to be an overt act of
relinquishment in perpetuity of all present and future rights to this
software under copyright law.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR
OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.

For more information, please refer to <http://unlicense.org/>


[dte]: https://github.com/craigbarnes/dte
[GNU nano]: http://www.nano-editor.org/
[nanorc]: http://www.nano-editor.org/dist/v2.3/nanorc.5.html
[theme.sed]: https://github.com/nanorc/nanorc/tree/master/theme.sed
[mixins.sed]: https://github.com/nanorc/nanorc/tree/master/mixins.sed
[mixins]: https://github.com/nanorc/nanorc/tree/master/mixins
[ERE]: http://pubs.opengroup.org/onlinepubs/009695399/basedefs/xbd_chap09.html#tag_09_04
