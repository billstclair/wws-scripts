# Bill St. Clair's Shell Scripts

This is a collection of shell scripts that I've written over the years. I never attempted to make them portable to different shells, so many will only work in <code>bash</code>. If you like one, and make it work in a different shell, please don't break it in <code>bash</code> and submit a pull request. My initials are "wws"; hence the repository name.

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

Useful to lispers. Deletes all fasl files in the current directory or any of its subdirectories, whether they're actually in this directory or they're in the sub-directory of ~/.cache into which ASDF will store them by default. Does NOT delete fasls from any directory named `quicklisp`, since you rarely want to force those to recompile. If you DO want to do that, `cd` into the `quicklisp` directory first:

```
cd ~/quicklisp
delfasls
```
