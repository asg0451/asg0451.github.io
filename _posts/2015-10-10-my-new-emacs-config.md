---
layout: post
title:  "My Emacs config and the use-package package"
date:   2015-10-10 22:10:20
categories: emacs
---

## use-package

I was on Reddit recently and stumbled across [oh-my-emacs][ome], which is a really cool-looking config system based on literate org-mode and `el-get`. This scared me a little bit, but, looking through the comments on the post, I found a package called `use-package` which I now use almost exclusively.

use-package is a less complex solution than el-get, and has a very nice declarative style in terms of declaring dependencies. An additional benefit for those like me who keep their config vc'd and instantiate it on new machines fairly often is that it will find and build any missing dependencies on startup without any complaints.[^1]

Here's an example from my init.el:

    (use-package visual-regexp
      :bind (("C-c r" . vr/replace)
             ("C-c q" . vr/query-replace)))

Packages required in this way will by default be autoloaded, which can speed up startup time if you're using a lot of packages. I could go on and on for at least fifteen minutes about this package, but instead your time would probably be better spent checking out their [github repo][up].

## Organization

The rest of my time spent on my config has been improving its organization: instead of one giant `init.el`, I have split it up into modules under a common directory. The modules include configurations for haskell development, org-mode[^2], helper functions, and general settings.

I have a lot of helper functions and general settings, some which I wrote/came up with, some of which I shamelessly stole from the internet. My current favourite function that I didn't write is `push-mark-no-activate` which I stole from [Mastering Emacs][me], which in its basic usage lets me mark a section, go somewhere else to look something up, and then pop back down when I am done.

## Functional functions
At a close second is the function `compose` which I got from [null program][np]. This and `apply-partially` (a builtin) make me feel more at home with emacs lisp (I had been missing it due to my haskell background). Together with `mapcar`, these functions let me code in elisp much more easily. As an example, this is the line that I use in init.el to load my modules:

    (mapc (dot 'load (ap 'concat modules-dir)) modules)

I have set `dot` and `ap` to be aliases for `compose` and `partially-apply`, respectively. First, I use `ap` to partially apply `concat` to the module directory prefix, which produces a function similar to `(lambda (&rest a) (mapcar 'concat modules-dir a))`. I then compose `load` over it and map it to the list of modules. This might sound unnecessarily complex, but hey it makes sense to me.

This is exactly how I would solve this problem in haskell, which made it a lot easier for me to write. Yes, I know that this is unnecessary and that I just saved myself some redundant code, but I like the look of it better this way. One of the main reasons that I use emacs is because of its extensibility, and treating its elisp configuration like a real language allows me to code better in it, I think.

&nbsp;



- - -

[^1]: if you have the `:ensure` flag set or the global variable `use-package-always-ensure` set to `t`
[^2]:   which I am not currently using to write this blog as it just seems too unwieldy to use with Jekyll.


[ome]:  https://github.com/xiaohanyu/oh-my-emacs
[up]:   https://github.com/jwiegley/use-package
[me]:   https://www.masteringemacs.org/article/fixing-mark-commands-transient-mark-mode
[np]: http://nullprogram.com/blog/2010/11/15/
