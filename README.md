# Bill St. Clair's Shell Scripts

This is a collection of little shell scripts that I've written over the years. I never attempted to make them portable to different shells, so many will only work in <code>bash</code>. If you like one, and make it work in a different shell, please don't break it in <code>bash</code> and submit a pull request. My initials are "wws"; hence the repository name.

All the script files named below are in the `bin` sub-directory of this one.

## xfiles

```
xfiles <pattern> <command> <arg> ...
```

is the same as:

```
find . -name "<pattern>" | xargs <command> <arg> ...
```

`lispfiles` passes `"*.lisp"` as the pattern. `erlfiles` passes `*.erl` as the pattern. Make your own.

I usually use it to grep source code:

```
lispfiles fgrep defpackage
```

## delfasls

Useful to lispers. Deletes all fasl files in the current directory or any of its subdirectories, whether they're actually in this directory or they're in the sub-directory of ~/.cache into which ASDF will store them by default. This will force ASDF (or Quicklisp) to recompie the corresponding source files.

Does NOT delete fasls from any directory named `quicklisp`, since you rarely want to force those to recompile. If you DO want to do that, `cd` into the `quicklisp` directory first:

```
cd ~/quicklisp
delfasls
```

## ip

If you're behind multiple routers, or you want to be sure that you're connected through a VPN, it can be useful to ask a remote web server what your IP address looks like to it. This script queries `http://billstclair.com/ip.php` for that. The `ip.php` script is also in the `bin` sub-directory here, in case you want to put it on your own web site instead of using mine.

The `ip` script echoes "expecting xx.xx.xx.xx". If you want to remind yourself of what your IP should be when you're connected through a VPN provider, you can change that to the actual IP you expect. Or just comment out the line.

## kerl_activate

A quick way to interact with the `activate` and `kerl_deactivate` scripts created by <a href='https://github.com/yrashk/kerl'>`kerl`</a>. Instead of:

```
. path/to/release/dir/<release-name>/activate
...
kerl_deactivate
```

You can do:

```
. kerl_activate <release-name>
...
kerl_deactivate
```

The `path/to/release/dir` is wired in to the `kerl_activate` script. You'll probably need to change it to match what you use.

`kerl_activate list` displays the names of the sub-directories in its wired-in release directory.

`. kerl_activate deactivate` is longhand for `kerl_deactivate`, there mostly for those of us who may have forgotten `kerl_deactivate`.

`kerl_activate` tells the kerl-generated `activate` script to enable the prompt that shows `(<build-name>)` at the beginning of your `PS1` shell prompt. Get rid of `KERL_ENABLE_PROMPT=y` if you don't like that.
