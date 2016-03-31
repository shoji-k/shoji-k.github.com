---
layout: post
title:  "serverspec"
date:   2016-03-31 16:00:00 +0900
categories: serverspec
---

[Serverspec - Home](http://serverspec.org/)

## install

$ bundle init  
$ vim Gemfile  

{% highlight ruby %}
source "https://rubygems.org"
gem 'serverspec'
{% endhighlight %}

$ bundle  
$ serverspec-init  

prepared sample spec  

$ rake spec ASK_SUDO_PASSWORD=1  




