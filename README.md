# Useful Little Shell Scripts

"I'm a useful engine." -- Thomas the Tank Engine

This is a collection of little shell scripts that I've written over the years. I never attempted to make them portable to different shells, so many will only work in <code>bash</code>. If you like one, and make it work in a different shell, please don't break it in <code>bash</code> and submit a pull request. My initials are "wws"; hence the repository name.

All the script files named below are in the `bin` sub-directory of this one.

## delfasls

Useful to lispers. Deletes all fasl files in the current directory or any of its subdirectories, whether they're actually in this directory or they're in the sub-directory of ~/.cache into which ASDF will store them by default. This will force ASDF (or Quicklisp) to recompile the corresponding source files.

Does NOT delete fasls from any directory named `quicklisp`, since you rarely want to force those to recompile. If you DO want to do that, `cd` into the `quicklisp` directory first:

```
cd ~/quicklisp
delfasls
```

## ip

If you're behind multiple routers, or you want to be sure that you're connected through a VPN, it can be useful to ask a remote web server what your IP address looks like to it. This script queries `http://billstclair.com/cgi-bin/ip.sh` for that. The `ip.sh` script is in the `aux` sub-directory here, in case you want to put it on your own web site instead of using mine. There's also an alternative, `ip.php`, if that's easier for you.

The `ip` script echoes "expecting xx.xx.xx.xx". If you want to remind yourself of what your IP should be when you're connected through a VPN provider, you can change that to the actual IP you expect. Or just comment out the line.

## rsyncit

Before the advent of `git`, I used to keep directories on my local machine in sync with web servers using `rsync`. I still do for image files. The problem was that I had to remember the `username@hostname:/path/to/webserver/directory` for a bunch of different sites. `rsyncit` allows you to put a `.sshdir` script in each directory you want to mirror somewhere. That script is responsible for echoing `username@hostname:/path/to/webserver/directory`. `rsyncit` calls `./.sshdir` and puts it in the right place for `rsync`.

If you give no arguments to `rsyncit`, it will sync the entire directory:

```
cd /local/path/to/webserver/images/dir
rsyncit
```

If you give arguments to `rsyncit` it will pass them to `rsync` with the output of the `./.sshdir` script added to the end:

```
rsyncit -av sunset-151002.jpg
```

There's a sample `.sshdir` script in the `aux` directory. Copy it to directories you want to sync, and change what it echoes appropriately.

## scpit

Like `rsyncit`, but uses `scp` to do the copying, instead of `rsync`. Uses the same `.sshdir` file to specify the remote location.

```
cd /local/path/to/webserver/images/dir
scpit sunset-151002.jpg
```

## xfiles

```
xfiles <pattern> <command> <arg> ...
```

is the same as:

```
find . -name "<pattern>" -print0 | xargs -0 <command> <arg> ...
```

`erlfiles` passes `*.erl` as the pattern.
`elmfiles` passes `*.elm` as the pattern.
 Make your own.

## lispfiles

```
lispfiles <arg> ...
```

is the same as:

```
find . -not -regex "\./quicklisp/.*" -and -name "*.lisp" -print0 | xargs -0 <arg> ...
```

I usually use it to grep source code:

```
lispfiles fgrep defpackage
```

`lispfiles` will NOT descend into a sub-directory named `quicklisp`, since you usually want to search YOUR code, not other people's libraries. You can `cd quicklisp` to find code there (unless you want to find code in `quicklisp/quicklisp`, in which case you'll need ANOTHER `cd quicklisp`).

## git-x-files

Like `xfiles`, but uses `git ls-files` for the full list, then filters it
with the first parameter, then pipes it through `xargs` and adds the rest of the parameters

`git-elm-files` and `git-js-files` use it.

## git-x-files-wc

Same as `git-x-files "$1" wc-l`

Useful to count lines in files of a partilar type in a git repository.

`git-elm-files-wc` and `git-js-files-wc` use it.

## git-x-files-wc-total

Same as `git-x-files-wc`, but prints only the total line at the end, not the individual files:

```
git-x-files-wc "$1" wc -l | grep total
```

`git-elm-files-wc-total` and `git-js-files-wc-total` use it.

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
