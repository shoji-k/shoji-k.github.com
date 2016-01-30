---
layout: post
title:  "capistrano for a cakephp app"
date:   2016-01-30 15:00:00 +0900
categories: ruby
---

reffer to  
[CakePHPで学ぶ継続的インテグレーション | インプレス](http://book.impress.co.jp/books/1114101035.php)  
[phpcibook/blogapp](https://github.com/phpcibook/blogapp)  

# conditions precedent
repository is prepared.  
login to development machine and prepare to be abled to git access to my repository.  

## how to connect to git

### register a ssh public key to github repository to be able to git clone

create ssh key  
$ ssh-keygen  

open open-key e.g. ./.ssh/id_rsa.pub and copy to clipboard   
and paste it to github repository setting  

### when fail to git clone in capistrano (cap production deploy)

reffer to:  
[capistrano - cap deploy で Permission denied(public key) が出る。 - Qiita](http://qiita.com/s_osa/items/c132096bf7592caeba73)  
[Git - ssh-addできなかったときへの対処 - Qiita](http://qiita.com/sshojiro/items/60982f06c1a0ba88c160)  

fail to add ssh id  

$ sudo ssh-add -l  
Could not open a connection to your authentication agent.  
$ ssh-add id_rsa  
Could not open a connection to your authentication agent.  
$ ssh-agent  
SSH_AUTH_SOCK=/tmp/ssh-0M9xl9eDoE1m/agent.2135; export SSH_AUTH_SOCK;  
SSH_AGENT_PID=2136; export SSH_AGENT_PID;  
echo Agent pid 2136;  
$ eval \`ssh-agent\`  
Agent pid 2138  
$ ssh-add ./id_rsa  
Identity added: ./id_rsa (./id_rsa)  

# prepare

$ mkdir deploy && cd deploy  
$ cap install  

{% highlight text %}
mkdir -p config/deploy
create config/deploy.rb
create config/deploy/staging.rb
create config/deploy/production.rb
mkdir -p lib/capistrano/tasks
create Capfile
Capified
{% endhighlight %}

prepare database setting for the deployment server.  
in this case, a php file for development.  

$ touch config/deploy/production.php  
$ vim config/deploy/production.php  

{% highlight php %}
<?php
Environment::configure('development', true, [
    'MYSQL_DB_HOST' => 'localhost',
    'MYSQL_USERNAME' => 'webapp',
    'MYSQL_PASSWORD' => 'passw0rd',
    'MYSQL_DB_NAME' => 'blog',
    'MYSQL_PREFIX' => '',
    'debug' => 0
], function() {
});
{% endhighlight %}

set server information to deploy  
change config/delopy/production.rb  
see below  
[blogapp/production.rb at master · phpcibook/blogapp](https://github.com/phpcibook/blogapp/blob/master/deploy/config/deploy/production.rb)  

set what to do in deploying  
change config/delopy.rb  
see below  
[blogapp/deploy.rb at master · phpcibook/blogapp](https://github.com/phpcibook/blogapp/blob/master/deploy/config/deploy.rb)  

implement to deploy  
$ cap production deploy:check  

if you fail to deploy, show in details  
$ cap production deploy:check --trace  

if you go back  
$ cap production deploy:rollback  

