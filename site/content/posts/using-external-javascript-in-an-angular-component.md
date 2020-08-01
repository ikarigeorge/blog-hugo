---
title: Using external javascript in an Angular component
date: 2020-08-01T21:18:17.509Z
tags:
  - angular
  - javascript
---
Sometimes is needed to use an external javascript code in an angular component, for example for some analytics, or to use an old library that is already part of the website.

If you try to add a *<script>* element inside an Angular component, it will not be inserted, since Angular ignores such elements. Adding it to the *index.html* file is not enough to be able to use it inside a component. 

In order to make use of such objects, they need to be "declared" using the `declare` keyword, so TypeScript understands there are some variables without a typing file.

Let's make an example, suppose you have jQuery added in the head of your index.html:

```
<head>
  <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" integrity="sha256-4+XzXVhsDmqanXGHaHvgh1gMQKX40OUvDEBTu8JcmNs=" crossorigin="anonymous"></script>
</head>
```

Now, imagine you have a component called example.component.ts, in order to use jQuery, you need to declare the $ variable:

```
import { Component, OnInit } from '@angular/core';

declare var $: any;

@Component({
  selector: 'example',
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.less']
})
export class ExampleComponent implements OnInit {

  constructor() { }

  ngOnInit() {
    const jqueryData = $('#my-example').data();
    console.log(jqueryData);
  }
}
```
