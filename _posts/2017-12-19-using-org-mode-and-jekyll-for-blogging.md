---
title: "Using org-mode and Jekyll for blogging"
date: 2017-12-19 12:22:00
layout: post
categories: Org-mode
---

I have been using org-mode for quite some time now, mostly for organizing my
time, agenda, and managing my to-do lists. There's plenty of blog posts and
tutorials written about org-mode by people with far more experience and
knowledge than me so I will not go into detail on how org-mode actually works. I
will also assume that there's at least some superficial knowledge on how
org-mode works by the reader.

I have also been using Jekyll for blogging for quite some time now, so finding a
way to blog through org-mode seems logical to me. It's the Emacs way!

The first thing I did, naturally is try to find some plugin for either Jekyll or
org-mode to make it possible to write Jekyll posts from org-mode. While there
are plenty of plugins to choose from, none of them really worked for me so I set
out to find a solution by myself.

The solution, as it turns out, is quite simple (at least for my needs). Jekyll
reads markdown files by default to create blog posts by compiling them to HTML.
So I only need a way to compile org files into markdown and add the YAML
frontend on top. Compilation to markdown is achieved through
[org-md](http://orgmode.org/manual/Markdown-export.html) and the built-in
[org-publish](http://orgmode.org/worg/org-tutorials/org-publish-html-tutorial.html)
feature. In order to use org-publish though, you first have to be set it up. So
I fired up Emacs and opened my `.emacs` file and added the following snippet:

    (setq org-publish-project-alist
          '(
            ("org-codepenguin"
             :base-directory "~/various/essays"
             :base-extension "org"
    
             :publishing-directory "~/thecodepenguin.github.io/_posts"
             :recursive f
             :publishing-function org-md-publish-to-md
             :headline-levels 4
             :html-extension "md"
             :body-only t)
            ("codepenguin" :components ("org-codepenguin"))
            ))

That might seem like gibberish for some, especially since I have not explained
my directory setup yet, so let's break this down.

I keep all the essays I am writing or have already written into a
"~/various/essays" directory. The website files are kept in
"~/thecodepenguin.github.io/" and Jekyll puts all posts into the "_posts" folder
inside the project directory.

The idea is to compile an org file from "~/various/essays" into a markdown file
and save it to "~/thecodepenguin.github.io/_posts". 

Now let's break down the above emacs-lisp code.

`base-directory` and `publishing-directory` are rather self-explanatory, they
set up the source and destination directories respectively. `recursive` is set
to false to avoid compiling every file and folder in the source directory (you
can set it to `t` if you want to compile everything).

This sets up the "compile to markdown" part, but there's still the YAML frontend
to set up. I do that directly into the file I am writing.

First, I use `#+options: toc:nil` to disable the table of contents that org-md
automatically generates when compiling. Then, in order to generate the YAML
frontend "as-is" I put it inside a source-code block:

    #+BEGIN_SRC markdown
    ---
    title: Using org-mode with Jekyll for blogging
    date: 2017-12-19 22:00:00
    layout: post
    categories: Org-mode
    ---

    #+END_SRC

This automatically generates the new post for me. Once I am done, I press `, e
e`, follow the instructions in the buffer that pops up and I am done. Jekyll
takes care of the rest.

I don't think that the system is perfect and there's certainly still room for
improvement, but for now it does everything I want it to do and it works for me.
