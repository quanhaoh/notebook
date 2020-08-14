### robotparser.RobotFileParser(url='')

用于解析robots.txt的类

**方法**

#### set_url(url)

#### read()
读取robots.txt文件内容

#### can_fetch(useragent, url)
测试useragent是否能读取url，可以返回True，否则返回False

#### mtime()
返回上一次读取robots.txt文件的时间，用于长时间的爬虫读取

#### modified()
将上一次读取robots.txt文件的时间设为当前时间

```
import urllib.robotparser
rp = urllib.robotparser.RobotFileParser()
rp.set_url("http://www.musi-cal.com/robots.txt")
rp.read()
rp.can_fetch("*", "http://www.musi-cal.com/cgi-bin/search?city=San+Francisco")
#False
rp.can_fetch("*", "http://www.musi-cal.com/")
#True
```
