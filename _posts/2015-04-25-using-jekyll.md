---
layout: post
title: " using jekyll"
description: ""
category: tools
tags: [github,jekyll]
---
{% include JB/setup %}

###step1: Register On Github
see [Github](https://www.github.com) for more information

###step2 简单的办法：使用Jekyll Bootstrap
1. Create a New Repository
2. Install Jekyll-Bootstrap
3. gem install jekyll
4. jekyll serve
5. create a post: rake post title="Hello World" 
6. 修改主题：[theme](http://jekyllbootstrap.com/usage/jekyll-theming.html)
7. 修改navigation的排序方式： 

    在navigation的页面头部添加index: index_order , 修改_includes/themes/your_theme/default.html,添加
`{ % include JB/sort_collection collection=site.pages sort_by="index" % }
{ % assign pages_list = sort_result % }` ，注意'{ %'或'% }'中间无空格
    

see [JekyllBootstrap](http://jekyllbootstrap.com/usage/jekyll-quick-start.html) for more information


###step2 复杂的办法:使用 bundler/github-pages,有助于了解jekyll 
    #change sources
    sudo gem sources --remove http://rubygems.org/
    sudo gem sources -a http://ruby.taobao.org/
    sudo gem sources --list
    
    #update gem
    sudo gem update --system
    
    #install bundler
    sudo gem install bundler -V
    
    #install github-pages
    sudo gem install github-pages
    
    #generate Gemfile
    cat <<EOF >>Gemfile
    source 'http://ruby.taobao.org'
    gem 'github-pages'"
    EOF

###碰到的问题
主要是网络连接上的问题,将gem source改为使用淘宝的源之后，问题基本都解决：

1. Gem::RemoteFetcher::FetchError: Errno::ECONNRESET: Connection reset by peer - SSL\_connect (https://rubygems.org/gems/RedCloth-4.2.9.gem)

    *__修改Gemfile__:  source 'https://rubygems.org' to  source 'http://rubygems.org'*
2. Operation timed out - connect(2) (https://rubygems.org/latest\_specs.4.8.gz)

    *__修改sourse__：sudo gem install bundler --http-proxy --source http://rubygems.org -V*


###Refer
* [jekyll-bootstrap](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)
* [github-jekyll-help](https://help.github.com/articles/using-jekyll-with-pages/)
* [jekyll-docs](http://jekyllrb.com/docs/home/)
* [mojombo-github-io](https://github.com/mojombo/mojombo.github.io)
* [jekyll-ubuntu setup help](http://trefoil.github.io/2013/10/05/jekyll.html)
* [markdown](http://daringfireball.net/projects/markdown/syntax)
