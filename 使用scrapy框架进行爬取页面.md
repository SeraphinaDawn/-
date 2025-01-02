# 使用scrapy框架进行爬取页面



## 1.创建项目

```css
scrapy startproject <项目名> ----创建一个项目
cd <项目名> ---- 切换目录
scrapy genspider top250 movie.douban.com ---可以自行创建
<!---第三行代码解释--->
scrapy genspider <name> <domain>

```

**`<name>`**: 爬虫名称

- 用来标识这个爬虫的名称，也是运行爬虫时使用的名称。
- 例如 `top250` 是这个爬虫的名称，运行爬虫时你会用以下命令：

- **`<domain>`**: 目标域名
  - 指定爬虫允许抓取的域名范围，它会被自动填充到生成的爬虫文件的 `allowed_domains` 属性中。
  - 例如这里的 `movie.douban.com`，它告诉 Scrapy 只抓取 `movie.douban.com` 子域下的内容。

这会在项目的 `spiders/` 文件夹下生成一个名为 `top250.py` 的爬虫文件，其初始内容大致如下

```python
import scrapy

class Top250Spider(scrapy.Spider):
    name = 'top250'  # 爬虫名称
    allowed_domains = ['movie.douban.com']  # 限制爬取的域名范围
    start_urls = ['http://movie.douban.com/']  # 起始 URL

    def parse(self, response):
        pass  # 用于编写解析逻辑

```

#### 代码解释

- **`name`**: 爬虫名称，由你在命令中指定的 `<name>` (`top250`) 填充。
- **`allowed_domains`**: 爬取范围，限制爬虫只抓取指定域名下的内容，由 `<domain>` (`movie.douban.com`) 填充。
- **`start_urls`**: 爬虫的起始 URL，默认使用 `<domain>`，你可以手动修改为更具体的地址，如 `https://movie.douban.com/top250`。

## 2.项目结构

```bash
myproject/
    scrapy.cfg          # 配置文件
    myproject/
        __init__.py     # 包初始化文件
        items.py        # 定义爬取的数据结构
        middlewares.py  # 中间件定义
        pipelines.py    # 数据处理管道
        settings.py     # 项目设置
        spiders/        # 存放爬虫代码
            __init__.py
```

**下面可能会有些许断层但无大碍,跟着大致写就好了**:

## 3.抓取数据需求

### 数据需求

- **电影名称**: 标题
- **导演与演员**: 提取导演和演员信息
- **电影评分**: 星级评分
- **首次上演日期**: 上映年份
- **评价人数**: 用户评价数
- **电影总结**: 简要介绍
- **海报封面地址**: 图片 URL
- **详情页地址**: 每部电影详情页链接



## 4.代码分析

可以看到我们的所有需要的内容都在`ol class=grid view`包裹的`li`下

![image-20250101162745056](https://gitee.com/ActonT/pic-go_img/raw/master/image-20250101162745056.png)

这里为我们的第一个内容![image-20250101162812674](https://gitee.com/ActonT/pic-go_img/raw/master/image-20250101162812674.png)

我们在详细的点开看看,根据下面<span style="color:#FF0000;">**`第一张图`**</span>我们进行获取详细的内容通过<span style="color:#FF0000;">**`第二张图`**</span>来获取`xpath`

### 第一张图

![image-20250101163331724](https://gitee.com/ActonT/pic-go_img/raw/master/image-20250101163331724.png)

### 第二张图

![image-20250101163605975](https://gitee.com/ActonT/pic-go_img/raw/master/image-20250101163605975.png)

### 第三张图片

![image-20250101163818752](https://gitee.com/ActonT/pic-go_img/raw/master/image-20250101163818752.png)

后面依次类推

### 4.1如何查找到我们需要爬取的元素呢?

我们回看上面的**<span style="color:#FF0000;">`第二张照片`</span>**进行一个一个的查早比如下面的动图,我需要爬取<span style="color:#FF0000;">**`有多少个人进行了评价`**</span>

![爬虫](https://gitee.com/ActonT/pic-go_img/raw/master/爬虫.gif)

```python
//*[@id="content"]/div/div[1]/ol/li[2]/div/div[2]/div[2]/div/span[4]
```

出来的内容如上, <span style="color:#FF0000;">**但是这并不是完美的**,</span>为什么呢?

原因如下:

- `div/div[1]/ol/li[2]`这里的`li`标签显示的是`li[2]`,代表了这个是当前电影的评价

- 如果我们想获取,全部的评价,就不能这么干,需要把`li[2]`这里的`2`给删掉改为:

  - ```python
    //*[@id="content"]/div/div[1]/ol/li/div/div[2]/div[2]/div/span[4]
    ```

- 便捷写法,因为我们开始代码会进行写一个方法

  - ```python
        def parse(self, response):
            movies = response.xpath('//*[@id="content"]/div/div[1]/ol/li')
    ```

  - 这里明显的已经写了`//*[@id="content"]/div/div[1]/ol/li`这个页面的所有`li`标签,
    所以我们循环的代码里面可以不写前面的东西了直接写

    ```python
    ./div/div[2]/div[1]/a/span[2]
    这里使用了绝对路劲了前面的也省略了
    ```

    



## 5.在`spiders`文件下创建爬虫代码

创建一个名为`douban_spider.py`的代码文件

下面有个模仿<span style="color:#FF0000;">**`人类`**</span>去请求网站我就不多说了,都是一样的套用就好

```python
from scrapy.spiders import Spider
from scrapy.http import Request
from scrapyspider.items import DoubanMovieItem


class DoubanMovieTop250Spider(Spider):
    name = 'douban_movie_top250'
    # 自定义请求头
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.143 Safari/537.36',
    }

    # 自定义 start_requests 方法
    def start_requests(self):
        url = 'https://movie.douban.com/top250'
        # 使用 Request，添加自定义的 headers
        yield Request(url, headers=self.headers)

        def parse(self, response):
            movies = response.xpath('//*[@id="content"]/div/div[1]/ol/li')
            for movie in movies:
                # 创建一个新的 DoubanMovieItem 实例
                item = DoubanMovieItem()

                # 电影标题
                item['title'] = movie.xpath('./div/div[2]/div[1]/a/span[1]/text()').get()
                item['english_title'] = movie.xpath('./div/div[2]/div[1]/a/span[2]/text()').get()

                # 电影导演与演员
                director_and_actors = movie.xpath('./div/div[2]/div[2]/p[1]/text()').getall()
                # 清洗导演与演员数据
                item['director_and_actors'] = [info.strip() for info in director_and_actors if info.strip()]

                # 电影评分
                item['rating'] = movie.xpath('./div/div[2]/div[2]/div/span[@class="rating_num"]/text()').get()

                # 电影评价人数
                item['review_count'] = movie.xpath('./div/div[2]/div[2]/div/span[4]/text()').re_first(r'\d+')

                # 电影总结
                item['summary'] = movie.xpath('./div/div[2]/div[2]/p[2]/span/text()').get(default="N/A")

                # 海报地址
                item['poster_url'] = movie.xpath('./div/div[1]/a/img/@src').get()

                # 电影详情页
                item['detail_page'] = movie.xpath('./div/div[1]/a/@href').get()

                # 返回数据
                yield item
```

### 5.1执行代码

> 这里需要在终端来进行执行代码,需要在你当前目录的根目录下进行下面代码

```python
scrapy crawl douban_movie_top250 -o douban_top250.csv
```

- 这里`scrapy crawl <为你的项目名>`<img src="https://gitee.com/ActonT/pic-go_img/raw/master/image-20250101190247870.png" alt="image-20250101190247870" style="zoom:50%;" />
- `-o douban_top250.csv` 这里为你要爬出来导出的文件夹名字,并以csv来保存

> 通过上面的完善代码,恭喜你已经完成爬取的第一步
>
> 但是你会发现只有25页,可我们要爬取的是250页才对啊,这这么办?
>
> ![嘿嘿嘿](https://gitee.com/ActonT/pic-go_img/raw/master/CAACAgUAAxkBAUkrNWd1IYTwiPzSvWmQxZOH3dckaED5AAJKBwAC_t2IV9tMiQgm.gif)
>
> 我也不知道什么问题

## 6.进行最后优化爬取250页

> 有两种方法
>
> 1. 在页面中找到下一页的地址；
> 2. 自己根据URL的变化规律构造所有页面地址。
>
> 一般情况下我们使用<span style="color:#FF0000;">**第一种方法**</span>，<span style="color:#FF0000;">**第二种方法**</span>适用于页面的下一页地址为JS加载的情况。今天我们只说第一种方法。 首先利用Chrome浏览器的开发者工具找到下一页的地址

**先根据下面的gif动图进行获取到地址**:

![爬虫2](https://gitee.com/ActonT/pic-go_img/raw/master/爬虫2.gif)

**在代码结尾添加如下代码**:

```python
next_url = response.xpath('//span[@class="next"]/a/@href').get()
if next_url:
    # 构造下一页的完整 URL
    next_url = response.urljoin(next_url)
    yield Request(next_url, headers=self.headers, callback=self.parse)
```

**递归翻页逻辑**：

- 使用 `response.xpath('//span[@class="next"]/a/@href').get()` 提取下一页的相对链接。
- 使用 `response.urljoin(next_url)` 将相对链接转换为完整的绝对 URL。

**停止条件**：

- 当最后一页没有下一页按钮时，`response.xpath('//span[@class="next"]/a/@href').get()` 返回 `None`，递归停止。

**确保请求头携带**：

- 在每次请求中都附加 `headers`，防止因爬虫检测而被封禁。

**结束**:![嘿嘿嘿嘿](https://gitee.com/ActonT/pic-go_img/raw/master/CAACAgUAAxkBAUkrUGd1I44oO_Z0xZbsOUIMc5UmcQmCAAKYDgAC1kLhVpIl24YB.gif)

## 7.完整代码

**`douban_spider.py`**:

```python
from scrapy.spiders import Spider
from scrapy.http import Request
from scrapyspider.items import DoubanMovieItem


class DoubanMovieTop250Spider(Spider):
    name = 'douban_movie_top250'
    # 自定义请求头
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.143 Safari/537.36',
    }

    # 自定义 start_requests 方法
    def start_requests(self):
        url = 'https://movie.douban.com/top250'
        # 使用 Request，添加自定义的 headers
        yield Request(url, headers=self.headers)

    def parse(self, response):
        movies = response.xpath('//*[@id="content"]/div/div[1]/ol/li')
        for movie in movies:
            # 创建一个新的 DoubanMovieItem 实例
            item = DoubanMovieItem()

            # 电影标题
            item['title'] = movie.xpath('./div/div[2]/div[1]/a/span[1]/text()').get()
            item['english_title'] = movie.xpath('./div/div[2]/div[1]/a/span[2]/text()').get()

            # 电影导演与演员
            director_and_actors = movie.xpath('./div/div[2]/div[2]/p[1]/text()').getall()
            # 清洗导演与演员数据
            item['director_and_actors'] = [info.strip() for info in director_and_actors if info.strip()]

            # 电影评分
            item['rating'] = movie.xpath('./div/div[2]/div[2]/div/span[@class="rating_num"]/text()').get()

            # 电影评价人数
            item['review_count'] = movie.xpath('./div/div[2]/div[2]/div/span[4]/text()').re_first(r'\d+')

            # 电影总结
            item['summary'] = movie.xpath('./div/div[2]/div[2]/p[2]/span/text()').get(default="N/A")

            # 海报地址
            item['poster_url'] = movie.xpath('./div/div[1]/a/img/@src').get()

            # 电影详情页
            item['detail_page'] = movie.xpath('./div/div[1]/a/@href').get()

            # 返回数据
            yield item
        # 找到下一页的地址
        next_url = response.xpath('//span[@class="next"]/a/@href').get()
        if next_url:
            # 构造下一页的完整 URL
            next_url = response.urljoin(next_url)
            yield Request(next_url, headers=self.headers, callback=self.parse)
```

**`items.py`**:

```python
# Define here the models for your scraped items
#
# See documentation in:
# https://docs.scrapy.org/en/latest/topics/items.html

import scrapy


class DoubanMovieItem(scrapy.Item):
    title = scrapy.Field()  # 电影名称
    english_title = scrapy.Field()  # 英文电影名称
    director_and_actors = scrapy.Field()  # 导演与演员信息
    rating = scrapy.Field()  # 电影评分
    # release_date = scrapy.Field()  # 首次上演日期
    review_count = scrapy.Field()  # 评价人数
    summary = scrapy.Field()  # 电影总结
    poster_url = scrapy.Field()  # 海报封面地址
    detail_page = scrapy.Field()  # 每部电影的详情页链接
    pass

```

## 8.报错问题

![image-20250101191704581](https://gitee.com/ActonT/pic-go_img/raw/master/image-20250101191704581.png)

一个字别管用终端启动就好

```python
scrapy crawl douban_movie_top250 -o douban_top250.csv
```

