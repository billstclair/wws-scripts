# Bill St. Clair's Shell Scripts

This is a collection of shell scripts that I've written over the years. I never attempted to make them portable to different shells, so many will only work in <code>bash</code>. If you like one, and make it work in a different shell, please don't break it in <code>bash</code> and submit a pull request. My initials are "wws"; hence the repository name.

## xfiles

```
xfiles &lt;pattern> <command> <arg> ...
```

is the same as:

```
find . -name "&lt;pattern>" | xargs <command> <arg> ...
```

`lispfiles` passes `"*.lisp"` as the pattern. `erlfiles` passes `*.erl` as the pattern. Make your own.
