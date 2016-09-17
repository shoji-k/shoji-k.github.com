---
layout: post
title:  "capistrano"
date:   2016-09-17 13:00:00 +0900
categories: deploy
---

[capistrano/capistrano: Remote multi\-server automation tool](https://github.com/capistrano/capistrano)

# install

$ bundle init  
$ vim Gemfile

{% highlight ruby %}
source "https://rubygems.org"

group :development do
  gem "capistrano", "~> 3.6"
end
{% endhighlight %}

$ bundle install

$ bundle exec cap -T

でコマンド一覧を見ることができます

コマンドの確認をしてみます

$ vim config/deploy.rb

{% highlight ruby %}
namespace :test do
    task :task1 do
        p "call task1"
    end
end
{% endhighlight %}

$ bundle exec cap production test:task1

エラーが出る..

{% highlight text %}
Stage not set, please call something such as `cap production deploy`, where production is a stage you have defined.
{% endhighlight %}

と思ったら Capfile のある場所で実行しないとだめなだけでした

productionのところは環境名ですが、
config/deploy/**.rb
とファイルがあればいいようです

roleごとにやる場合

{% highlight ruby %}
namespace :test do
    task :task1 do
        on roles(:app) do
            p "call task1"
        end
    end
end
{% endhighlight %}


デプロイ先をセット

$ vim config/deploy.rb

{% highlight ruby %}
set :deploy_to, '/var/www/html/sample'
{% endhighlight %}

$ vim config/deploy/production.rb

{% highlight ruby %}
server 'sample.co.jp', user: 'user', roles: %w{web db}
{% endhighlight %}

デプロイしてみると

$ bundle exec cat production deploy

こんな感じでリモートサーバーへデプロイされます

{% highlight text %}
/var/www/html/sample
├── current -> /var/www/html/sample/releases/20160917063933
├── releases
│   └── 20160917063933
│       ├── index.html
├── repo
├── revisions.log
└── shared
{% endhighlight %}

複数サーバーに一気にデプロイするとか、キャッシュを消したり、DB migrateしたりとかする際に便利そうです
ロールバックできるのもよさそう


