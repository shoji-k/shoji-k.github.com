---
layout: post
title:  "javascript tips"
date:   2016-01-28 15:00:00 +0900
categories: javascript
---

# get json throught xhr

Getting local json file works on firefox, does't work in chrome and ie.

{% highlight js %}
var xobj = new XMLHttpRequest();
xobj.overrideMimeType("application/json");
xobj.open('GET', './json/'+this.kind+'.json', true);
xobj.onreadystatechange = function () {
  if (xobj.readyState == 4 && xobj.status == "200") {
    this.data = JSON.parse(xobj.responseText);
  }
}.bind(this);
xobj.send(null);
{% endhighlight %}


