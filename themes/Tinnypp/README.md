##Tinnypp

###About

This is a clean theme for [hexo](https://github.com/hexojs/hexo)

It is based on [Tinny](https://github.com/zhanglun/hexo-theme/tree/master/Tinny).

I have made some optimizations on Tinny and you can see the [demo](http://levonlin.github.io/Tinnypp/).

###Installation

####Install
	
	$ cd themes
	$ git clone git@github.com:levonlin/Tinnypp.git

####Enable

Modify the <code>theme</code> setting in <code>_config.yml</code> of the blog folder.

    theme: Tinnypp

####Update

	$ cd themes/Tinnypp
	$ git pull

###Configuration

Modify settings in the file <code>Tinnypp/_config.yml</code>.

Tinnypp :

1.uses [swiftype](https://swiftype.com/) as a site's search engine.If you want to use swiftype,please change the last <code>&lt;script&gt;&lt;/script&gt;</code> tag to your swiftype install code in <code>Tinnypp/layout/_partial/head.ejs</code> . :)

2.has set /atom.xml as an rss source.To use rss,you can install hexo rss plugin

	$npm install hexo-generator-feed

Then add 

	plugins:
	- hexo-generator-feed

in <code>_config.yml</code> of the blog folder.

For more detials,you can vist [My blog](http://www.levonlin.info/tags/Tinnypp/) .And if you have any problems,you can create [issues](https://github.com/levonlin/Tinnypp/issues) .

##觉得看中文爽的小伙伴看下面~

###关于

本主题是一个[hexo](https://github.com/hexojs/hexo)的小清新主题。就是在[Tinny](https://github.com/zhanglun/hexo-theme/tree/master/Tinny)基础上做了些我自己认为的优化（折腾）。具体效果可见[demo](http://levonlin.github.io/Tinnypp/)。

###装载本主题

####安装
	
	$ cd themes
	$ git clone git@github.com:levonlin/Tinnypp.git

####载入

去你博客文件夹下的<code>_config.yml</code>文件编辑<code>theme</code>设定如下：

    theme: Tinnypp

####升级

	$ cd themes/Tinnypp
	$ git pull

###设置本主题

编辑<code>Tinnypp/_config.yml</code>文件即可。

本主题:

1.使用[swiftype](https://swiftype.com/)作为站内搜索引擎，如果你想使用这个酷炫的搜索，请在<code>Tinnypp/layout/_partial/head.ejs</code>里将最后一个<code>&lt;script&gt;&lt;/script&gt;</code>标签改为你的swiftype安装代码。:)

2.使用[多说](http://duoshuo.com/)作为评论系统，可在<code>Tinnypp/_config.yml</code>中开启或关闭。此外如果你想显示评论者的[user agent](http://zh.wikipedia.org/wiki/%E7%94%A8%E6%88%B7%E4%BB%A3%E7%90%86)，可直接修改<code>themes/Tinnypp/layout/_partial/after_footer.ejs</code>里的<code>ds.src = '//static.duoshuo.com/embed.js';</code>为<code>'ds.src = '<%- config.root %>js/embed.js'';</code>，并在你的多说里添加这个[自定义css](https://github.com/levonlin/Tinnypp/blob/master/source/css/%E5%A4%9A%E8%AF%B4%E8%87%AA%E5%AE%9A%E4%B9%89.css)即可。若还想显示你的“博主头衔”，请记得编辑<code>Tinnypp/source/js/embed.js</code>里的<code>e.user_id == your duoshuo ID</code>。

3.已经设置/atom.xml为rss源。要使用rss,先安装hexo的rss插件：

	$npm install hexo-generator-feed

然后添加 

	plugins:
	- hexo-generator-feed

到博客文件夹下的<code>_config.yml</code>。

4.中文显示在博客文件夹下的<code>_config.yml</code>设置

	language: zh-CN

即可。但若想在文章发布日期有符合中文习惯的显示，请在<code>_config.yml</code>里继续设置

	month_format: YYYY年 MMM
	date_format: YYYY年 MMM D日 
	time_format: H:mm:ss

更多细节，各位亲们可以访问[我的博客](http://www.levonlin.info/tags/Tinnypp/)。如果觉得这个主题有什么值得吐槽和改进的地方，也欢迎各位亲们给我发[issues](https://github.com/levonlin/Tinnypp/issues)。