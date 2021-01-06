---
title: "Installing a List of Extensions in Visual Studio Code"
date: 2017-02-25
tags: [vscode]
---

I’ve been setting up a new OS X installation and wanted to quickly get Visual Studio Code set back up. Atom has a really handy command line utility, `apm`, that lets you do useful things like export a list of extensions and reinstall them elsewhere by passing in that list as a command line argument.

Unfortunately, while Visual Studio Code’s command line utility allows you to get a list of extensions with `code --list-extensions` which you can pipe into a text file, it doesn’t appear to have any way to automatically install the extensions to that file.

Fortunately, a minute with Google and Stack Overflow turns up [this very helpful answer](http://stackoverflow.com/questions/13939038/how-do-you-run-a-command-for-each-line-of-a-file) to run a command for each line in a text file. From there, some quick trial and error got me to this:

```bash
while read in; do code --install-extension "$in"; done < ~/vscode-extensions.txt
```

Not quite as convenient as Atom’s solution, but a nice way to make sure you don’t overlook anything.

