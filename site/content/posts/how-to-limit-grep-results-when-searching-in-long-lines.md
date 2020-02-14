---
title: How to limit grep results when searching in long lines
date: 2020-02-14T20:37:25.949Z
tags:
  - linux
  - snippets
---
The \`grep\` command is very useful to print lines containing certain patterns. But sometimes we may be looking for something in lines that are extremely long, for example in a minified javascript file.

There is a way to limit the number of characters before and after the pattern we are searching for, using the extended regexp and telling grep to gives us only the match.

For example, suppose we have a bundle.min.js file, and we are looking for the word "foobar", if we just want to print 20 characters before and after, then we execute:

```
 grep -E -o ".{0,20}foobar.{0,20}" bundle.min.js
```
