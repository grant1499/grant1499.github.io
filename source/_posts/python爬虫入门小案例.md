---
title: python爬虫入门小案例
tags:
  - 爬虫
categories:
  - Python
mathjax: true
abbrlink: '38740982'
date: 2021-12-07 16:31:45
---

# 案例一：爬取豆瓣top250影片相关信息

使用`requests`库爬取。

<!--more-->

IDLE输入：

```python
>>> import requests
>>> import bs4
>>> headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36 SE 2.X MetaSr 1.0"}
>>> res=requests.get("https://movie.douban.com/top250",headers=headers)
>>> soup = bs4.BeautifulSoup(res.text,"html.parser")
>>> targets = soup.find_all("div",class_="hd")
>>> for each in targets:
	print(each.a.span.text)
```

主程序：

```python
import requests
import bs4
import re

def open_url(url):
    # 使用代理
    # proxies = {"http": "127.0.0.1:1080", "https": "127.0.0.1:1080"}
    headers = {'user-agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/57.0.2987.98 Safari/537.36'}

    # res = requests.get(url, headers=headers, proxies=proxies)
    res = requests.get(url, headers=headers)

    return res

def find_movies(res):
    soup = bs4.BeautifulSoup(res.text, 'html.parser')
 
    # 电影名
    movies = []
    targets = soup.find_all("div", class_="hd")
    for each in targets:
        movies.append(each.a.span.text)

    # 评分
    ranks = []
    targets = soup.find_all("span", class_="rating_num")
    for each in targets:
        ranks.append(' 评分：%s ' % each.text)

    # 资料
    messages = []
    targets = soup.find_all("div", class_="bd")
    for each in targets:
        try:
            messages.append(each.p.text.split('\n')[1].strip() + each.p.text.split('\n')[2].strip())
        except:
            continue

    result = []
    length = len(movies)
    for i in range(length):
        result.append(movies[i] + ranks[i] + messages[i] + '\n')

    return result

# 找出一共有多少个页面
def find_depth(res):
    soup = bs4.BeautifulSoup(res.text, 'html.parser')
    depth = soup.find('span', class_='next').previous_sibling.previous_sibling.text

    return int(depth)

def main():
    host = "https://movie.douban.com/top250"
    res = open_url(host)
    depth = find_depth(res)

    result = []
    for i in range(depth):
        url = host + '/?start=' + str(25 * i)
        res = open_url(url)
        result.extend(find_movies(res))

    with open("豆瓣TOP250电影.txt", "w", encoding="utf-8") as f:
        for each in result:
            f.write(each)
    
if __name__ == "__main__":
    main()
```

# 案例二：爬取acwing上的刷题数量

主程序：

```python
import re
import requests
from lxml import etree


class Spider(object):
    def __init__(self):
        self.base_url = 'https://www.acwing.com/problem/{}/'

        self.headers = {
            'Cookie': 'csrftoken=sukdhRyZvZ5xIU4si62uFjnPrsTOB4tEHDoAzK9TmUA74TCVsF; sessionid='
            'y5owkpblzawdlw1p3rx91jwm6; file_2510567_readed=""; file_362295_readed=""; question_6210_readed=""; file_3495_readed=""; file_2897078_readed=""; file_2482882_readed=""; file_20826_readed=""; file_2917651_readed=""; file_17080_readed=""; file_03179_readed=""; question_623319_readed=""; question_623315_readed=""; file_2848720_readed=""; file_4818_readed=""; file_28216_readed=""; file_8708readed=""; file_2209490_readed=""; file_1111647_readed=""; file_421808_readed=""; file_1874010_readed=""; file_2312_readed=""; file_143921_readed=""; file_5296_readed=""; file_140683_readed=""; file_383397_readed=""; file_588001_readed=""; file_1120777_readed=""; file_144026_readed=""; file_70686_readed=""; file_1112271_readed=""; file_2331251_readed=""; file_2808_readed=""; file_144598_readed=""; file_70694_readed=""; file_2926390_readed=""; file_2926598_readed=""',
            'Referer': 'https://www.acwing.com/about/',
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
                        ' (KHTML, like Gecko) Chrome/93.0.4577.63 Safari/537.36'
        }
        self.count = 0
		# 注：上面的cookie和session要换成自己账号的，上面的跑不通的！
    def get_html(self, url):
        html = requests.get(
            url=url,
            headers=self.headers
        ).text

        return html

    def xpath_func(self, html):
        name_bds = '//tbody/tr[./td/span[@title="已通过这道题目"]]/td/a/text()'
        base_obj = etree.HTML(html)
        name_lists = base_obj.xpath(name_bds)
        L = []
        for i in name_lists:
            L.append(i.strip())
        return L

    def re_func(self, html, re_bds):
        pattern = re.compile(re_bds, re.S)
        re_list = pattern.findall(html)

        return re_list

    def parse_html(self, url):
        html = self.get_html(url)
        L = self.xpath_func(html)
        return L

    def run(self):
        warning = input('您马上就要爬取acwing了，看一下你的做题数，您的劳动成果将会在下面展示出来，确定要看吗？(Y/N)')
        if warning == 'Y':
            print('爬虫系统已经启动...正在努力抓取，请稍等....')
            print('+---------------------------------+')
            print('|            name                 |')
            print('+---------------------------------+')
            for i in range(1, 80):
                url = self.base_url.format(i)
                L = self.parse_html(url)
                for _ in L:
                    self.count += 1
                    print('|   ' + _)
            print('+---------------------------------+')
            print('经过您的不懈努力，您一共做了' + str(self.count) + '道题，继续努力！！')
        else:
            print('已经退出，你这个弱者')

if __name__ == '__main__':
    # spider = Spider()
    # spider.run()
    print("hello")
```

# 案例三：爬取微博热搜

主程序：

```python
import urllib.request #导入urllib.request库
import urllib.parse
from lxml import etree

#b = str(input("请输入："))   #提示用户输入信息，并强制类型转换为字符串型

def weibo() :
    url = 'https://s.weibo.com/top/summary?'
    a = urllib.request.urlopen(url)  #打开指定网址
    html = a.read()  #读取网页源码
    html = html.decode("utf-8")  #解码为unicode码
    # print(html)                #打印网页源码

    tree = etree.HTML(html)
    list = tree.xpath(u'//*[@id="pl_top_realtimehot"]/table/tbody/tr/td[@class="td-02"]')

    prefix = 'https://s.weibo.com'  # 微博域名
    weiboSummary = open("微博热搜.txt", 'w')  #打开并写入文件
    for index,item in enumerate(list):
        if index > 0:
            a_element = item.xpath('.//a')[0]
            title = a_element.text # 关键词
            href = urllib.parse.unquote(a_element.attrib.get('href')) # 链接
            href = href.replace("#", "%23") #此处是对链接中的#号做一个编码转换，否则无法跳转指定关键词链接
            hot = item.xpath('./span')[0].text #热度指数
            line = str(index) + "\t" + title + "\t" + hot + "\t" + prefix + href + "\n"
            print(line.replace("\n", ""))
            if href.find("javascript:void(0)") != -1:
                line = str(index) + "\t" + title + "\t" + hot
            #写入文件
            weiboSummary.write(line)
        else:
            title = '排名\t关键词\t热度\t链接\n'
            print(title.replace("\n",""))
            weiboSummary.write(title)

    weiboSummary.close()
    
#调用方法
weibo();
```

