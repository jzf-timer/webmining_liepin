# 20春_Web数据挖掘_期末项目

#  
> (数据加值宣言：本项目产出按XXX及XXX挖掘的关于YYY(例子: NPS)工作的数据，以解决NPS就业需求及特性的就业分析问题)
> 注. 需达成评价表格PRD1.考核内容：＂作者成功地把数据产品对加值（总结解决什么问题）的精确丶专业及中肯地总结表述于第一段"
* 运用 scrapy框架 挖取猎聘网中，一线城市 “北上广深的”外加新一线城市重庆的“新媒体运营人员”职位的详情页内容，并生成excel表格。有薪资-工作要求-学历-年龄等基本信息。

数据加值: 得到一线城市中具有代表性的新媒体运营人员职业的信息，解决数据分析师方向职业需求，得知薪资-要求等概况（相同或差异）



# 数据最小可用产品
> (MVP的数据加值)：需达成评价表格PRD2.考核内容：＂作者成功地具体表述数据产品的数据类型及内容如何构成最小可用产品MVP的核心价值（具体什么数据解决什么问题）
*  得到一线城市和新一线城市的新媒体运营职业的信息，解决新媒体运营方向职业需求，得知薪资-要求等概况（相同或差异）。让想要去一线或者新一线城市就职新媒体运营的人群可以知道需要什么样的条件，薪资概况等等。可以更加有目的性的寻找职位，并且增加一定的求职成功概率。

# 挖掘Query参数
 Query参数包括：1. Jobtitle 职称 2. Salary 工资 3. Area 地区 4.  AcademicDegree 学历 5.Experience 经验 6.Company 公司


```def start_requests(self):
        allinfo=[]
        #以下是关于上海的循环
        headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3314.0 Safari/537.36 SE 2.X MetaSr 1.0'}
        for i in range(9):
            SHurl='https://www.liepin.com/zhaopin/?compkind=&dqs=020&pubTime=&pageSize=40&salary=&compTag=&sortFlag=15&degradeFlag=0&compIds=&subIndustry=&jobKind=&industries=&compscale=&key=%E6%96%B0%E5%AA%92%E4%BD%93%E8%BF%90%E8%90%A5&siTag=qkuPMtyyPWyGJLVm3Ykn1A%7Er3i1HcfrfE3VRWBaGW6LoA&d_sfrom=search_fp&d_ckId=c51a068c5cb658f7f4040175ba945596&d_curPage=2&d_pageSize=40&d_headId=4107d9372116a7333a50ba34629aa075&curPage={}'.format(i)
            response=requests.get(SHurl,headers=headers)
            # print(response.text)
            response=etree.HTML(response.text)
                                #//*[@id="sojob"]/div[3]/div/div[1]/div[1]/ul/li/div/div[1]
            divs=response.xpath('//*[@id="sojob"]/div[3]/div/div[1]/div[1]/ul/li/div')#div列表
            print(divs)
            for div in divs:
                Jobtitle = div.xpath('./div[1]/h3/a/text()')  # 工作标题
                Jobtitle = Jobtitle[0].replace('\r', '').replace('\n', '').replace('\t', '')
                Salary = div.xpath('./div[1]/p[1]/span[1]/text()')  # 薪资
                Area = div.xpath('./div[1]/p[1]/a/text()')  # 地区
                # print(Area)
                if Area != []:
                    AcademicDegree = div.xpath('./div[1]/p[1]/span[2]/text()')  # 学历
                    Experience = div.xpath('./div[1]/p[1]/span[3]/text()')  # 经验
                    Company = div.xpath('./div[2]/p/a/text()')  # 公司
                else:
                    Area = div.xpath('./div[1]/p[1]/span[2]/text()')  # 地区
                    AcademicDegree = div.xpath('./div[1]/p[1]/span[3]/text()')  # 学历
                    Experience = div.xpath('./div[1]/p[1]/span[4]/text()')  # 经验
                    Company = div.xpath('./div[2]/p/a/text()')  # 公司
                print(Jobtitle, Salary[0], Area[0], AcademicDegree[0], Experience[0], Company[0])
                allinfo.append([Jobtitle, Salary[0], Area[0], AcademicDegree[0], Experience[0], Company[0]])
```
# 思路方法及具体执行
1. 先是确定好我要解决的问题——“老一线城市”和新一线代表城市中的新媒体运营职业详情
2. 选取猎聘网为数据挖掘——www.liepin.com
3.[先用request加xpath 爬取这个新媒体运营人员的薪资水平和职业详情](https://github.com/GREGJASON/webmining_liepin/blob/master/%E7%BF%BB%E9%A1%B5%E6%95%B0%E6%8D%AE.ipynb)
4.[用之前0爬取到详情信息形成翻页数据](https://github.com/GREGJASON/webmining_liepin/blob/master/20%E6%98%A5_Web%E6%95%B0%E6%8D%AE%E6%8C%96%E6%8E%98_final_%E6%96%B0%E5%AA%92%E4%BD%93%E8%BF%90%E8%90%A5_%E7%BF%BB%E9%A1%B5.xlsx)
5. [然后用scrapy框架爬取相关的城市新媒体运营职位的页面链接和首页概况的年龄、薪资等基本信息](https://github.com/GREGJASON/webmining_liepin/blob/master/LiePing/spiders/LP.py)
6. [然后用scrapy框架将上一步的各个职位招聘的详情页信息爬取形成excel表格](https://github.com/GREGJASON/webmining_liepin/blob/master/LiePing/%E7%8C%8E%E8%81%98%E6%96%B0%E5%AA%92%E4%BD%93%E8%BF%90%E8%90%A5.xls)
7.方法选择：初始 使用request模块+xpath 是为了可以爬取现有的新媒体运营职业相关信息 然后构建单页数据和多页数据模板 供后面的scrapy框架代码提供一定基础
            后面选择scrapy框架 是为了可以更好地将老一线城市 北上广深和新一线城市中的代表城市重庆中新媒体运营人员的职业信息
 8.单页数据：
 
![输入图片说明] (https://upload-images.jianshu.io/upload_images/9443754-fd0ef5ec3b0ff65a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
# 心得总结及感谢


特别感谢中大南方[廖汉腾主任](https://www.baidu.com/link?url=a1iZrLywMyppofbh53HPSWH5c3pWyxrV2TaVnnC1U8XdhGtcXHNH-E3grALR5bLAzNQyBnsd-r0DoTahxBgqGK&wd=&eqid=8b1070bf001a044f000000065f12d982)与 **许智超** 老师在这个学期对我们的Web数据挖掘课程教学指导。由于上半学期疫情的影响，本课程结合线上+线下的课程完成教学学习，老师们的耐心讲解让我掌握到了数据挖掘的技术，也认识到了数据分析师的思维方式，这将对于我们日后做有关数据挖掘、数据分析的相关工作、任务时，带来很大的帮助！！

>参考文档:
[初窥Scrapy](https://scrapy-chs.readthedocs.io/zh_CN/latest/intro/overview.html)
