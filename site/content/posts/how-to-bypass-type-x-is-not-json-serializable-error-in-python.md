---
title: How to bypass "type X is not JSON serializable" error in python
date: 2020-01-09T22:07:07.684Z
tags:
  - python
  - snippets
---
When working with dictionaries in python, sometimes the attributes type are not serializable. Specially with dates and times, which is a common attribute when building API answers for example.

The problem comes when we need to convert it as a JSON string. Since some types are not serializable. Take the following snippet:

```
import json
from datetime import date

today = date.today()
dict = { 
  "today": today,
  "foo": "bar"
}
print(json.dumps(dict))

```

This code will fail with the following error: TypeError: Object of type date is not JSON serializable

To avoid this, we can pass the "default" argument to the dumps method:

```
print(json.dumps(dict, default=str))
```

This way, in case some type cannot be serialized, it will be converted to string.
