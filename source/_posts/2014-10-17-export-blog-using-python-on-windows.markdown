---
layout: post
title: "用Python编写博客导出工具"
date: 2014-10-12 08:35:02 +0800
comments: true
categories: [工具软件, 软件开发]
tags: [Python, 工具]
description: 用Python编写博客导出工具
keywords: 博客导出工具, Python 
---

### 写在前面的话
我在 github 上用 octopress 搭建了[个人博客](http://kesalin.github.io/)，octopress 使用Markdown语法编写博文。之前我在CSDN博客上也写过不少的技术博文，都说自己的孩子再丑也是个宝，所以就起了把CSDN博客里面的文章导出到个人博客上的念头。刚开始想找个工具把CSDN博客导出为xml或文本，然后再把xml或文本转换为Markdown博文。可惜搜了一下现有博客导出工具，大部分要收费才能将全部博文导出为xml格式，所以就只好发明轮子了：写个工具将全部博文导出为Markdown博文（也是txt格式的）。

我将详细介绍这个工具的编写过程，希望没有学习过编程的人也能够学会一些简单的Python语法来修改这个脚本工具，以满足他们将其他类型的博客导出为文本格式。这也是我第一次学习和使用Python，所以相信我，你一定也可以将自己的博客导出为想要的文本格式。

本文源代码在这里：[ExportCSDNBlog.py](https://github.com/kesalin/PythonSnippet/blob/master/ExportCSDNBlog.py)

<!--more-->

考虑到大部分非程序员使用Windows系统，下面将介绍在Windows下如何编写这个工具。

### 下载工具
在 Windows 下安装Python开发环境（Linux/Mac下用pip安装相应包即可，程序员自己解决咯）：

**Python 2.7.3**  
请安装这个版本，更高版本的Python与一些库不兼容。  
[下载页面](https://www.python.org/download/releases/2.7.3/)  
下载完毕双击可执行文件进行安装，默认安装在C:\Python2.7。

**six**  
[下载页面](https://pypi.python.org/pypi/six)
下载完毕，解压到Python安装目录下，如C:\Python2.7\six-1.8.0目录下。

**BeautifulSoup 4.3.2**  
[下载页面](http://www.crummy.com/software/BeautifulSoup/bs4/download/)，
下载完毕，解压到Python安装目录下，如C:\Python2.7\BeautifulSoup目录下。

**html5lib**  
[下载页面](https://pypi.python.org/pypi/html5lib)
下载完毕，解压到Python安装目录下，如C:\Python2.7\html5lib-0.999目录下。

### 安装工具
Windows下启动命令行，依次进入如下目录，执行setup.py install进行安装：  

    C:\Python2.7\six-1.8.0>setup.py install  
	C:\Python2.7\html5lib-0.999>setup.py install  
	C:\Python2.7\BeautifulSoup>setup.py install  

### 参考文档
[Python 2.X文档](https://docs.python.org/2/)  
[BeautifulSoup文档](http://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html)  
[正则表达式文档](http://www.regexlab.com/zh/regref.htm)  
[正则表达式在线测试](http://tool.oschina.net/regex)

### 用到的Python语法
这个工具只用到了一些基本的Python语法，如果你没有Python基础，稍微了解一下如下博文是很有好处的。

* string: 字符串操作，参考[python: string的操作函数](http://www.cnblogs.com/sevenyuan/archive/2010/12/10/1902145.html)
* list: 列表操作，参考[Python list 操作](http://www.cnblogs.com/zhengyuxin/articles/1938300.html)
* dictionary: 字典操作，参考[Python中dict详解](http://www.cnblogs.com/yangyongzhi/archive/2012/09/17/2688326.html)
* datetime: 日期时间，参考[python datetime处理时间](http://www.cnblogs.com/lhj588/archive/2012/04/23/2466653.html)

<br/>

### 编写博客导出工具

#### 分析 
首先来分析这样一个工具的需求：

	导出所有CSDN博客文章为Markdown文本。

这个总需求其实可以分两步来做：

	* 获得CSDN博客文章
	* 将文章转换为Markdown文本

针对第一步：如何获取博客文章呢？

打开任何一个CSDN博客，我们都可以看到下方的页面导航显示“XXX条数据 共XXX页 1 2 3 ... 尾页”，我们从这个地方入手考虑。每个页面上都会显示属于该页的文章标题及文章链接，如果我们依次访问这些页面链接，就能从每个页面链接中找出属于该页面的文章标题及文章链接。这样所有的文章标题以及文章链接就都获取到了，有了这些文章链接，我们就能获取对应文章的html内容，然后通过解析这些html页面来生成相应Markdown文本了。

#### 实现
从上面的分析可以看出，首先我们需要根据首页获取所有的页面链接，然后遍历每一个页面链接来获取文章链接。

* 获取页面链接的代码：

``` python 获取所有的页面的 url https://github.com/kesalin/PythonSnippet/blob/master/ExportCSDNBlog.py View Source
		def getPageUrlList(url):
		    # 获取所有的页面的 url
		    request = urllib2.Request(url, None, header)
		    response = urllib2.urlopen(request)
		    data = response.read()

		    #print data
		    soup = BeautifulSoup(data)

		    lastArticleHref = None
		    pageListDocs = soup.find_all(id="papelist")
		    for pageList in pageListDocs:
		        hrefDocs = pageList.find_all("a")
		        if len(hrefDocs) > 0:
		            lastArticleHrefDoc = hrefDocs[len(hrefDocs) - 1]
		            lastArticleHref = lastArticleHrefDoc["href"].encode('UTF-8')

		    if lastArticleHref == None:
		        return []
		    
		    #print " > last page href:" + lastArticleHref
		    lastPageIndex = lastArticleHref.rfind("/")
		    lastPageNum = int(lastArticleHref[lastPageIndex+1:])
		    urlInfo = "http://blog.csdn.net" + lastArticleHref[0:lastPageIndex]

		    pageUrlList = []
		    for x in xrange(1, lastPageNum + 1):
		        pageUrl = urlInfo + "/" + str(x)
		        pageUrlList.append(pageUrl)
		        log(" > page " + str(x) + ": " + pageUrl)

		    log("total pages: " + str(len(pageUrlList)) + "\n")
		    return pageUrlList
``` 

参数 url = "http://blog.csdn.net/" + username，即你首页的网址。通过urllib2库打开这个url发起一个web请求，从response中获取返回的html页面内容保存到data中。你可以被注释的 print data 来查看到底返回了什么内容。

有了html页面内容，接下来就用BeautifulSoup来解析它。BeautifulSoup极大地减少了我们的工作量。我会详细在这里介绍它的使用，后面再次出现类似的解析就会从略了。soup.find_all(id="papelist") 将会查找html页面中所有id="papelist"的tag，然后返回包含这些tag的list。对应 CSDN 博文页面来说，只有一处地方：

``` html
	<div id="papelist" class="pagelist">
		<span> 236条数据  共12页</span>
		<strong>1</strong> 
		<a href="/kesalin/article/list/2">2</a>
		<a href="/kesalin/article/list/3">3</a>
		<a href="/kesalin/article/list/4">4</a> 
		<a href="/kesalin/article/list/5">5</a> 
		<a href="/kesalin/article/list/6">...</a> 
		<a href="/kesalin/article/list/2">下一页</a> 
		<a href="/kesalin/article/list/12">尾页</a>
	</div>
``` 

好，我们获得了papelist 的tag对象，通过这个tag对象我们能够找出尾页tag a对象，从这个tag a解析出对应的href属性，获得尾页的编号12，然后自己拼出所有page页面的访问url来，并保存在pageUrlList中返回。page页面的访问url形式示例如下：

 	> page 1: http://blog.csdn.net/kesalin/article/list/1

* 根据page来获取文章链接的代码：

``` python
		def getArticleList(url):
		    # 获取所有的文章的 url/title
		    pageUrlList = getPageUrlList(url)
		    
		    articleListDocs = []

		    strPage = " > parsing page {0}"
		    pageNum = 0
		    global gRetryCount
		    for pageUrl in pageUrlList:
		        retryCount = 0
		        pageNum = pageNum + 1
		        pageNumStr = strPage.format(pageNum)
		        print pageNumStr

		        while retryCount <= gRetryCount:
		            try:
		                retryCount = retryCount + 1
		                time.sleep(1.0) #访问太快会不响应
		                request = urllib2.Request(pageUrl, None, header)
		                response = urllib2.urlopen(request)
		                data = response.read().decode('UTF-8')
		                
		                #print data
		                soup = BeautifulSoup(data)
		                
		                topArticleDocs = soup.find_all(id="article_toplist")
		                articleDocs = soup.find_all(id="article_list")
		                articleListDocs = articleListDocs + topArticleDocs + articleDocs
		                break
		            except Exception, e:
		                print "getArticleList exception:%s, url:%s, retry count:%d" % (e, pageUrl, retryCount)
		                pass
		    
		    artices = []
		    topTile = "[置顶]"
		    for articleListDoc in articleListDocs:
		        linkDocs = articleListDoc.find_all("span", "link_title")
		        for linkDoc in linkDocs:
		            #print linkDoc.prettify().encode('UTF-8')
		            link = linkDoc.a
		            url = link["href"].encode('UTF-8')
		            title = link.get_text().encode('UTF-8')
		            title = title.replace(topTile, '').strip()
		            oneHref = "http://blog.csdn.net" + url
		            #log("   > title:" + title + ", url:" + oneHref)
		            artices.append([oneHref, title])

		    log("total articles: " + str(len(artices)) + "\n")
		    return artices
``` 

从第一步获得所有的page链接保存在pageUrlList中，接下来就根据这些page 页面来获取对应page的article链接和标题。关键代码是下面这三行：

    topArticleDocs = soup.find_all(id="article_toplist")
    articleDocs = soup.find_all(id="article_list")
    articleListDocs = articleListDocs + topArticleDocs + articleDocs

从page的html内容中查找置顶的文章（article_toplist）以及普通的文章（article_list）的tag对象，然后将这些tag保存到articleListDocs中。

article_toplist示例：(article_list的格式是类似的)

``` html
    <div id="article_toplist" class="list">
        <div class="list_item article_item">
            <div class="article_title">   
                <span class="ico ico_type_Original"></span>
                <h1>
                    <span class="link_title">
                    <a href="/kesalin/article/details/10474007">
                    <font color="red">[置顶]</font>
                    招聘：有兴趣做一个与Android对等的操作系统么？
                    </a>
                    </span>
                </h1>
            </div>
            ... ...
        </div>
        ... ...
    </div>
``` 

然后遍历所有的保存到articleListDocs里的tag对象，从中解析出link_title的span tag对象保存到linkDocs中；然后从中解析出链接的url和标题，这里去掉了置顶文章标题中的“置顶”两字；最后将url和标题保存到artices列表中返回。artices列表中的每一项内容示例：

   title:招聘：有兴趣做一个与Android对等的操作系统么？  
   url:http://blog.csdn.net/kesalin/article/details/10474007

* 根据文章链接获取文章html内容并解析转换为Markdown文本

``` python
		def download(url, output):
		    # 下载文章，并保存为 markdown 格式
		    log(" >> download: " + url)

		    data = None
		    title = ""
		    categories = ""
		    content = ""
		    postDate = datetime.datetime.now()
		    
		    global gRetryCount
		    count = 0
		    while True:
		        if count >= gRetryCount:
		            break
		        count = count + 1
		        try:
		            time.sleep(2.0) #访问太快会不响应
		            request = urllib2.Request(url, None, header)
		            response = urllib2.urlopen(request)
		            data = response.read().decode('UTF-8')
		            break
		        except Exception,e:
		            exstr = traceback.format_exc()
		            log(" >> failed to download " + url + ", retry: " + str(count) + ", error:" + exstr)
		            pass

		    if data == None:
		        log(" >> failed to download " + url)
		        return

		    #print data
		    soup = BeautifulSoup(data)

		    topTile = "[置顶]"
		    titleDocs = soup.find_all("div", "article_title")
		    for titleDoc in titleDocs:
		        titleStr = titleDoc.a.get_text().encode('UTF-8')
		        title = titleStr.replace(topTile, '').strip()
		        #log(" >> title: " + title)

		    manageDocs = soup.find_all("div", "article_manage")
		    for managerDoc in manageDocs:
		        categoryDoc = managerDoc.find_all("span", "link_categories")
		        if len(categoryDoc) > 0:
		            categories = categoryDoc[0].a.get_text().encode('UTF-8').strip()
		        
		        postDateDoc = managerDoc.find_all("span", "link_postdate")
		        if len(postDateDoc) > 0:
		            postDateStr = postDateDoc[0].string.encode('UTF-8').strip()
		            postDate = datetime.datetime.strptime(postDateStr, '%Y-%m-%d %H:%M')

		    contentDocs = soup.find_all(id="article_content")
		    for contentDoc in contentDocs:
		        htmlContent = contentDoc.prettify().encode('UTF-8')
		        content = htmlContent2String(htmlContent)

		    exportToMarkdown(output, postDate, categories, title, content)
``` 

同前面的分析类似，在这里通过访问具体文章页面获得html内容，从中解析出文章标题，分类，发表时间，文章内容信息。然后把这些内容传递给函数exportToMarkdown，在其中生成相应的Markdown文本文件。值得一提的是，在解析文章内容信息时，由于html文档内容有一些特殊的标签或转义符号，需要作特殊处理，这些特殊处理在函数htmlContent2String中进行。目前只导出了所有的文本内容，图片，url链接以及表格都没有处理，后续我会尽量完善这些转换。

``` python
		def htmlContent2String(contentStr):
		    patternImg = re.compile(r'(<img.+?src=")(.+?)(".+ />)')
		    patternHref = re.compile(r'(<a.+?href=")(.+?)(".+?>)(.+?)(</a>)')
		    patternRemoveHtml = re.compile(r'</?[^>]+>')

		    resultContent = patternImg.sub(r'![image_mark](\2)', contentStr)
		    resultContent = patternHref.sub(r'[\4](\2)', resultContent)
		    resultContent = re.sub(patternRemoveHtml, r'', resultContent)
		    resultContent = decodeHtmlSpecialCharacter(resultContent)
		    return resultContent
``` 

目前仅仅是删除所有的html标签，并在函数decodeHtmlSpecialCharacter中转换转义字符。

* 生成Markdown文本文件

``` python
		def exportToMarkdown(exportDir, postdate, categories, title, content):
		    titleDate = postdate.strftime('%Y-%m-%d')
		    contentDate = postdate.strftime('%Y-%m-%d %H:%M:%S %z')
		    filename = titleDate + '-' + title
		    filename = repalceInvalidCharInFilename(filename)
		    filepath = exportDir + '/' + filename + '.markdown'
		    log(" >> save as " + filename)

		    newFile = open(unicode(filepath, "utf8"), 'w')
		    newFile.write('---' + '\n')
		    newFile.write('layout: post' + '\n')
		    newFile.write('title: \"' + title + '\"\n')
		    newFile.write('date: ' + contentDate + '\n')
		    newFile.write('comments: true' + '\n')
		    newFile.write('categories: [' + categories + ']' + '\n')
		    newFile.write('tags: [' + categories + ']' + '\n')
		    newFile.write('description: \"' + title + '\"\n')
		    newFile.write('keywords: ' + categories + '\n') 
		    newFile.write('---' + '\n\n')
		    newFile.write(content)
		    newFile.write('\n')
		    newFile.close()
``` 

生成Markdown文本文件就很简单了，在这里我需要生成github page用的Markdown博文形式，所以内容如此，你可以根据你的需要修改为其他形式的文本内容。
