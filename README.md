# BaiduTongjiExporter

由于百度统计的如下政策修改，开发本工具用于数据定时导出.

> 尊敬的百度统计用户您好，基础统计报告对于分析云站点最早查询时间将调整为2年，其余站点最早查询时间调整为1年，为避免数据丢失，建议您于2022.4.12前完成历史数据的下载或截图备份。如有疑问或更久的数据存储时长需求，您可发邮件至ext_tongji_reply@baidu.com 或 点击咨询 与我们联系，由此给您造成的不便请您谅解。 

下面是定时同步策略：

每日获取【北京时间次日 12 时】：
* 网站概况(趋势数据)
* 网站概况(地域分布)
* 网站概况(来源网站、搜索词、入口页面、受访页面)
* 趋势分析【今日，昨日对比】
* 实时访客
* 全部来源【今日，昨日对比】
* 搜索引擎【今日，昨日对比】
* 搜索词【今日，昨日对比】
* 外部链接【今日，昨日对比】
* 受访页面【今日，昨日对比】
* 入口页面【今日，昨日对比】
* 受访域名【今日，昨日对比】
* 地域分布【今日，昨日对比】
* 地域分布(按国家)【今日，昨日对比】

每周获取【次周一 12 时】：
* 网站概况(趋势数据)【周度】
* 网站概况(地域分布)【周度】
* 网站概况(来源网站、搜索词、入口页面、受访页面)【周度】
* 趋势分析【本周，上周对比】
* 全部来源【本周，上周对比】
* 搜索引擎【本周，上周对比】
* 搜索词【本周，上周对比】
* 外部链接【本周，上周对比】
* 受访页面【本周，上周对比】
* 入口页面【本周，上周对比】
* 受访域名【本周，上周对比】
* 地域分布【本周，上周对比】
* 地域分布(按国家)【本周，上周对比】

每月获取【北京时间次月 1 日 12 时】：
* 网站概况(趋势数据)【月度】
* 网站概况(地域分布)【月度】
* 网站概况(来源网站、搜索词、入口页面、受访页面)【月度】
* 趋势分析【本月，上月对比】
* 全部来源【本月，上月对比】
* 搜索引擎【本月，上月对比】
* 搜索词【本月，上月对比】
* 外部链接【本月，上月对比】
* 受访页面【本月，上月对比】
* 入口页面【本月，上月对比】
* 受访域名【本月，上月对比】
* 地域分布【本月，上月对比】
* 地域分布(按国家)【本月，上月对比】

脚本还支持通过指定时间段下载除实时访客外的历史数据。

## 使用方法

处理北京时间昨天的数据：

```
> python .\cli.py fetch --help
Usage: cli.py fetch [OPTIONS]

Options:
  -t, --access-token TEXT  Access token
  -p, --path TEXT          Path to save report
  -d, --date TEXT          Date to get report, in form of YYYY-MM-DD of
                           Beijing time
  --help                   Show this message and exit.
```

处理指定时间的历史数据：

```
> python .\cli.py fetch-all --help
Usage: cli.py fetch-all [OPTIONS] [%Y-%m-%d] [%Y-%m-%d]

  Fetch all history reports by indicating start date and end date in form of
  YYYY-MM-DD

Options:
  -t, --access-token TEXT  Access token
  -p, --path TEXT          Path to save report
  --help                   Show this message and exit.
```

安装依赖：

```bash
pip install -r requirements.txt
```
