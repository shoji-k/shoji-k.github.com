---
layout: post
title:  "git with gpg"
date:   2016-04-11 18:00:00 +0900
categories: git
---

# on ubuntu

gpg is installed by default  
gpg2 is not installed by default  
gpg2 is gpg v2  

refer to: [GitHubでGPGにより署名されたcommitにバッジが表示されるようになったので設定してみる - Qiita](http://qiita.com/prince_0203/items/ef0e12f2f6d150ff0485)  

# create gpg key

$ gpg --gen-key  

follow the instruction  
wait for a long time  

$ gpg --list-keys  

{% highlight text %}
pub   4096R/xxxxxxxx 2016-04-10
uid           sample foobar <sample@sample.com>
sub   4096R/yyyyyyyy 2016-04-10
{% endhighlight %}

xxxxxxxx is the id  

show public key  
$ gpg --armor --export <コピーした鍵のID>  

copy and paste in the github setting  

$ git config --global gpg.program gpg  
$ git config --global user.signingkey <コピーした鍵のID>  
$ git config --global commit.gpgsign true  

# copy gpg key

copy .gnupg directory  

