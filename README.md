![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 将sp_dbmmonitorresults放入表格
#### Put sp_dbmmonitorresults Into a Table
**发布-日期: 2015年7月20日 (评论)**

![#](images/##############?raw=true "#")

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
下面的逻辑（logic）将获取sp_dbmmonitorresults的结果然后放入表格中。 这将涵盖服务器上所有的镜像数据库。


## English
Here’s some SQL logic that will take the results of sp_dbmmonitorresults and put into a table. This will cover ALL mirrored databases on the server.

---
## Logic
```SQL
use master;
set nocount on
 
declare @get_mirror_status_of_all_dbs varchar(max)
set @get_mirror_status_of_all_dbs = ''
select  @get_mirror_status_of_all_dbs = @get_mirror_status_of_all_dbs + 'exec msdb..sp_dbmmonitorresults ''' + sd.name + ''';' + CHAR(10)
from    sys.databases sd inner join sys.database_mirroring sdm on sd.database_id = sdm.database_id where    sd.database_id &gt; 4 and sdm.mirroring_guid is not null order by name asc
 
if OBJECT_ID('tempdb..#mirror_stats') is not null
drop table #mirror_stats
create table #mirror_stats
(
[stat_id] int identity(1,1)
,   [database_name] sysname
,   [role] int
,   [mirroring_state] int
,   [witness_status] int
,   [log_generation_rate] int
,   [unsent_log] int
,   [send_rate] int
,   [unrestored_log] int
,   [recovery_rate] int
,   [transaction_delay] int
,   [transactions_per_sec] int
,   [average_delay] int
,   [time_recorded] datetime
,   [time_behind] datetime
,   [local_time] datetime
)
 
insert into #mirror_stats
(
[database_name]
,   [role]
,   [mirroring_state]
,   [witness_status]
,   [log_generation_rate]
,   [unsent_log]
,   [send_rate]
,   [unrestored_log]
,   [recovery_rate]
,   [transaction_delay]
,   [transactions_per_sec]
,   [average_delay]
,   [time_recorded]
,   [time_behind]
,   [local_time]
)
 
exec    (@get_mirror_status_of_all_dbs)
 
select * from #mirror_stats
 
drop table #mirror_stats


```



[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

