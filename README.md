# BaiduTongjiExporter

**本项目已适配 Github Actions，Fork 本项目即可使用，具体使用方法见下文。**

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

## CLI 使用方法

**所有的 SECRETS / TOKEN 都可以通过环境变量传递**

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

用例：获取 `2020-12-20` 到 `2022-04-10` 的数据，指定路径为 `./reports`

```bash
python cli.py fetch-all 2020-12-20 2022-04-10 -p ./reports -t <ACCESS_TOKEN>
```

安装依赖：

```bash
pip install -r requirements.txt
```

刷新 Token (具体内容详见百度统计文档):

```
> python .\cli.py refresh-token --help
Usage: cli.py refresh-token [OPTIONS]

  refresh access token: output refresh token in the first line, access token
  in the second line

Options:
  -r, --refresh-token TEXT  Refresh token
  -k, --api-key TEXT        API key
  -s, --secret-key TEXT     Secret key
  --help                    Show this message and exit.
```

## GitHub Actions 使用方法

Github Actions 有三个：

* `refresh_token`: 用于自动刷新百度统计 `ACCESS_TOKEN` 和 `REFRESH_TOKEN`，UTC 时间每周一凌晨执行
* `cron_export`: 用于每日自动导出数据，会自动检测是否为周初以及月初，并执行相应的自动导出任务
* `manual_export`: 用于手动导出指定日期，用于补救某些日期 action failed 但是没有重试，或者 cron 因为系统错误未正确发起的情况

首先需要明确：导出的数据并非存储在本仓库，我自己是另行指定了一个 Private Repo。为了让自己的 GitHub 的 Activity History 不至于太难看，我还建了一个 Github 的小号，充当 bot。Action 可以指定 bot 的用户名与邮箱。

下面是仓库的 secrets 配置详情：

* `BAIDU_TONGJI_ACCESS_TOKEN`: 百度统计 `ACCESS_TOKEN`.（新建即可，会自动刷新）
* `BAIDU_TONGJI_REFRESH_TOKEN`: 百度统计 `REFRESH_TOKEN`. 见[文档](https://tongji.baidu.com/api/manual/)
* `BAIDU_TONGJI_API_KEY`: 百度统计 `API_KEY`. 见[文档](https://tongji.baidu.com/api/manual/)
* `BAIDU_TONGJI_SECRET_KEY`: 百度统计 `SECRET_KEY`. 见[文档](https://tongji.baidu.com/api/manual/)
* `REPO_ACCESS_TOKEN`: `Github` 的 `Personal Access Token`，用于自动更新 `Secrets` 与拉取 private repo. 见[文档](https://docs.github.com/en/enterprise-cloud@latest/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)
* `REPO_NAME`: 存放数据的 repo 名称，格式：`<username>/<repo>`
* `GIT_EMAIL`: `push` 导出数据的账户邮箱
* `GIT_USERNAME`: `push` 导出数据的账户用户名

Fork 本项目后配置完 secrets 即可使用。
