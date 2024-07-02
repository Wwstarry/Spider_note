# Python爬虫学习笔记

## 爬虫开发使用的开发环境

**开发环境：**windows、Mac、linux

**编辑器：**Pycharm

**网页下载：**requests

**网页解析：**BeautifulSoup/bs4

**动态网页下载：**Selenium

## 简单的爬虫架构!
[1](https://github.com/Wwstarry/Spider_note/blob/main/1.jpg)

## 网页下载器（使用requests库）

**发送request请求（语法）：**requests.get/post(url,params,data,headers,timeout,verify,allow_redirects,cookies)

**url：**要下载的目标网页的URL

**params：**字典形式，设置URL后面的参数，比如id：123&name=xiaoming

**data：**字典或者字符串，一般用于POST方法时提交数据

**headers：**设置user-agent、refer等请求头

**timeout：**超时时间，单位是秒

**verify：**True/False，是否进行HTTP证书验证，默认是，需要自己设置证书地址

**allow_redirects：**True/False是否让requests作重定向处理，默认是

**cookies：**附带本地的cookies数据

**接收response响应（语法）：**

r = requests.get/post(url)

**相关指令：**

r.status_code  <!--查看状态码，如果等于200代表请求成功-->

r.encoding <!--可以查看当前编码，以及更改编码（requests会根据Headers推测编码，推测不到则设置为ISO-8859-1可能导致乱码）-->

r.text <!--查看返回的网页内容-->

r.headers <!--查看返回的HTTP的headers-->

r.url <!--查看实际访问的URL-->

r.content <!--以字节方式返回内容，比如用于下载图片-->

r.cookies <!--服务端要写入本地的cookies数据-->

## URL管理器（管理爬取的URL，防止重复和循环爬取，支持新增和取出URL）

[2](https://github.com/Wwstarry/Spider_note/blob/main/2.jpg)

**核心代码实现：**

`class UrlManager():`

​    `def __init__(self):`
​        `self.new_urls = set()`
​        `self.old_urls = set()`
​     `"""两个容器来装未爬取和已爬取的url"""`

​    `def add_new_url(self, url):`
​        `if url is None or len(url) == 0:`
​            `return`
​        `if url in self.new_urls or url in self.old_urls:`
​            `return`
​        `self.new_urls.add(url)`
​    `"""新增url，该url在新旧url容器中均不存在，即不重复"""`

​    `def add_new_urls(self,urls):`
​        `if urls is None or len(urls) == 0:`
​            `return`
​        `for url in urls:`
​            `self.add_new_url(url)`
​    `"""实现url的批量添加"""`

​    `def has_new_url(self):`
​        `return len(self.new_urls)>0`
​    `"""判断是否还有未爬取的url"""`

​    `def get_url(self):`
​        `if self.has_new_url():`
​            `url = self.new_urls.pop()`
​            `self.old_urls.add(url)`
​            `return url`
​        `else:`
​            `return None`

## HTML简介     

**HTML是用标记标签来描述网页的一种超文本标记语言**，**标签成对出现，第一个标签为开始，第二个标签为结束。**

**标签：**h1~h4、div、p、a、img

**属性：**id、class、href

`<a href='baidu.html' class='article_link'>百度</a>`

**节点名称：**a

**节点属性：**href='baidu.html'/class='article_link'

**节点内容：**百度

## 网页解析器（使用Beautiful Soup库）

[3](https://github.com/Wwstarry/Spider_note/blob/main/3.jpg)

`from bs4 import BeautifulSoup`

`\#创建Beautiful Soup对象`
`soup = BeautifulSoup(html_doc, 'html.parser', fro,_encoding='utf8')`
`\#三个参数分别为HTML文档字符串、HTML解析器、HTML文档编码`

`\#搜索节点(find_all,find)`
`links = soup.find_all('div', class_='abc', string='Python')`
`\#查找所有标签为div,class为abc,文字为Python的节点`

`\#访问节点信息`
`for link in links:`
    `print(link.name)#获取查找到的节点的标签名称`
    `print(link['href'])#获取查找到的节点的href属性`
    `print(link.get_text())#获取查找到的节点的文字`
