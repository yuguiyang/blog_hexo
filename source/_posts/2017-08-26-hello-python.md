---
title: hello-python
date: 2017-08-26 15:42:07
tags: python
---
```python
# -*- coding: utf-8 -*-
"""
Created on Tue Aug 22 16:13:24 2017

@author: yuguiyang
"""

import os
import urllib
from bs4 import BeautifulSoup
import jieba
from wordcloud import WordCloud
import matplotlib.pyplot as plt

file_name = 'douban_movie_zhanlang.txt'

#根据URL，获取页面源码
def getHtml(url):
    req = urllib.request.Request(url)
    page = urllib.request.urlopen(req).read().decode('utf-8')
    
    return page

#删除文件
def clearFile(targetFile):
    if os.path.exists(targetFile):
        os.remove(targetFile)
    
#将数据保存到文件
def save2file(lines,targetFile):
    with open(targetFile,'a') as file:
        file.writelines(lines)

#解析当前页面        
def parseCurrentPage(html):
    soup = BeautifulSoup(html, "html.parser")
    
    #获取评论信息
    p_comments = soup.select('div#comments div.comment p')

    p_lines=[]
    for com in p_comments:
        p_lines.append(com.contents[0].strip()+'\n')
    
    #获取下一页信息，可以通过这个获取数据
    p_next = soup.select_one('div#paginator a.next')
    #print(p_next)
    print(p_next['href'])
    
    return p_lines
    
def showWordCloud(targetFile):
    #指定字体，是为了显示中文
    font = r'C:\Windows\Fonts\simsun.ttc'
    with open(targetFile) as file:
        comments = file.read()
        text_cut = jieba.cut(comments , cut_all=False)
        
        #指定需要提出的词语
        stopwords = {u'个',u'一个',u'这个',u'个人',u'不是',u'就是',u'一部',u'这部'
                     ,u'我们',u'所以',u'不会',u'这种',u'没有',u'各种',u'觉得'
                     ,u'真的',u'知道',u'还是',u'但是',u'可以',u'这么',u'因为',u'很多'}
        print('stopwords',stopwords)
        

        wordcloud = WordCloud(font_path=font,width=800,height=400,
                              background_color='white',stopwords=stopwords
                              ).generate(' '.join(text_cut))
        
        plt.imshow(wordcloud, interpolation='bilinear')
        plt.axis("off")
        
def main():   
    url = 'https://movie.douban.com/subject/26363254/comments?start={0}&limit=20&sort=new_score&status=P'
    
    clearFile(file_name)
    #直接遍历200条评论，这里要注意，超过多少页后，需要登录才可以，这里暂时还没有做，就到200
    for i in range(0,200,20):    
        print(url.format(i))
        html = getHtml(url.format(i))
        movie_comments = parseCurrentPage(html)
        save2file(movie_comments,file_name)
    
    #显示词云图
    showWordCloud(file_name)

if __name__=='__main__':
    main()
```
