---
layout: post
title:  "bootstrap"
date:   2016-10-20 13:00:00 +0900
categories: bootstrap
---

# install

[Download · Bootstrap](http://v4-alpha.getbootstrap.com/getting-started/download/)

I used npm.  

$ npm init
$ npm install bootstrap@4.0.0-alpha.4

$ vim .gitignore

{% highlight text %}
# Logs
logs
*.log
npm-debug.log*

# Grunt intermediate storage (http://gruntjs.com/creating-plugins#storing-task-files)
.grunt

# Dependency directories
node_modules

# Optional npm cache directory
.npm

# Optional eslint cache
.eslintcache
{% endhighlight %}

$ git init  
$ git add -A  
$ git commit -m 'first commit'

$ npm install --save-dev gulp  
$ npm install --save-dev gulp-sass  
$ npm install --save-dev gulp-autoprefixer  

prepare gulpfile.js  
$ vim gulpfile.js  

{% highlight text %}
var gulp = require('gulp');
var sass = require('gulp-sass');
var autoprefixer = require('gulp-autoprefixer');

gulp.task("compile", function () {
    gulp.src('./node_modules/bootstrap/scss/bootstrap.scss')
        .pipe(sass())
        .pipe(autoprefixer())
        .pipe(gulp.dest('css/'));
});

gulp.task("watch", function() {
  gulp.watch('./node_modules/bootstrap/scss/*.scss', ['compile']);
});

gulp.task("default", ['compile']);
{% endhighlight %}

output css  
$ gulp

see below  
[Bootstrap 4のCSSコンパイルタスクをGulpで管理する方法 \- Qiita](http://qiita.com/tonkotsuboy_com/items/564f9bb78d177483f83e)



