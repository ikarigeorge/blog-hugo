---
title: Adding timezones and substracting dates in python
date: 2020-10-11T13:52:25.073Z
tags:
  - python
---
Python's date and datetime objects can sometimes seem difficult to use when trying to calculate time deltas or when trying to use timezones. There are several libraries to deal with it, however in case no dependencies are required, you can do the following:

Create a datetime with a timezone:

To do so, the timedelta class receives as input the time difference, so for example, if you want to be in Europe/Berlin zone which is GMT+2, the following snippet would be used:
```
from datetime import datetime, timezone, timedelta
today = datetime.now(timezone(timedelta(hours=2)))
```


Substracting dates:

When making a substraction (or any other operation) between two dates or datetimes, python will give you a timedelta object, however you may want to have some time object or a date object. In order to do so, the `total_seconds` method can help you:

```
seconds = (datetime.now - myDate).total_seconds()
```
