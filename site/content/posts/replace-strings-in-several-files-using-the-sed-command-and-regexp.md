---
title: Replace strings in several files using the sed command and RegExp
date: 2020-02-10T15:39:25.588Z
tags:
  - linux
  - snippets
  - regexp
---
Sometimes it is necessary to replace some text inside a file using the `sed` command. But what if the text to search for is not fixed but has some pattern?

A regular expression can be used alongside sed 🙂. Let's suppose we have the following listA.txt and listB.txt

```
name=John,email=john@myurl.com
name=Eva,email=eva@myurl.com
name=Laurie,email=laurie@anotherurl.com
```
```
order=12324,email=john@myurl.com
order=45656,email=eva@myurl.com
```

If we want to replace the emails with a set of XXXs we could use the following shell script:

```
for i in list*.txt; do
    sed -i -r 's/\w+@[a-zA-Z_]+?\.[a-zA-Z]{2,3}\.?[a-zA-Z]{0,3}/XXXXX/g' $i
done
```
The result would be for example:
```
name=John,email=XXXXX
name=Eva,email=XXXXX
name=Laurie,email=XXXXX
```
