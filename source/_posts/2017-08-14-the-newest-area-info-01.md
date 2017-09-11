---
title: 最新行政区信息获取
date: 2017-08-14 21:59:00
categories:
- "随笔"
tags:
- "随笔"
---

这是之前记录的，这里顺便分享下。

之前想要获取官方的省市区的代码code，就找了下。

官方地址：[http://www.stats.gov.cn/tjsj/tjbz/xzqhdm/](http://www.stats.gov.cn/tjsj/tjbz/xzqhdm/)

![the-newest-area-info-01-01](http://7xl61k.com1.z0.glb.clouddn.com/the-newest-area-info-01-01.png-blog.photo)

我们可以直接将数据复制到Excel中，简单处理下，导入到数据库中

里面会有换行，先去个重，然后trim一下，再分列就可以了

![the-newest-area-info-01-02](http://7xl61k.com1.z0.glb.clouddn.com/the-newest-area-info-01-02.png-blog.photo)

![the-newest-area-info-01-03](http://7xl61k.com1.z0.glb.clouddn.com/the-newest-area-info-01-03.png-blog.photo)

最后，我们可以将数据组织成我们想要的维度表格式

--------------update at 2017-08-15

上面呢，我是直接在Excel里面生成insert脚本插入数据库的，但是目前的表使用起来应该不是很方便，这里我们改造下。

这个区域信息呢，是很常用的一张维度表，通常他可能会是这个样子的
``` sql
CREATE TABLE base.dm_area
(
  area_id integer,
  area_name character varying(50),
  province_id integer,
  province_name character varying(50),
  city_id integer,
  city_name character varying(50),
  country_id integer,
  country_name character varying(50),
  flevel integer
)
WITH (
  OIDS=FALSE
);
```

这里我们就简单处理下，变成这种格式。

其实一开始的时候，我们可以通过数据的格式区分他是省份、城市、还是区县，下面我们用的是一个套路

主要是观察法，因为这个code都是有规律的，所以我们处理起来方便很多

code一共是6位，前两位是省份、中间2位是城市，后2位是区县
``` sql
select *from public.b_area where area_id||'' like '00'
```

![the-newest-area-info-01-04](http://7xl61k.com1.z0.glb.clouddn.com/the-newest-area-info-01-04.png-blog.photo)

下面，我们来初始化数据
``` sql
INSERT INTO base.dm_area(
            area_id, area_name)
select *from  public.b_area;      


-- 初始化flevel 1:省份,2:城市,3:区县
update base.dm_area set flevel=1 where area_id||'' like '00';
update base.dm_area set flevel=2 where area_id||'' like '' and flevel is null;
update base.dm_area set flevel=3 where flevel is null;

-- 更新省份信息
update base.dm_area a 
set 
	province_id = b.area_id,
	province_name = b.area_name
from 
	base.dm_area b
where 
	left(a.area_id||'',2)=left(b.area_id||'',2) and b.flevel=1;

-- 更新城市信息
update base.dm_area a 
set 
	city_id = c.area_id,
	city_name = c.area_name	
from 
	base.dm_area c 
where 
	left(a.area_id||'',4)=left(c.area_id||'',4) and c.flevel=2;

-- 更新区县信息
update base.dm_area a 
set 
	country_id = area_id,
	country_name = area_name	
where 
	a.flevel=3;
```

到这里，flevel=3的数据已经没有问题了，再对省份、城市做些优化就行了
``` sql
-- 补全数据
update base.dm_area 
set 
	city_id=-area_id,city_name=area_name||'XT',
	country_id=-area_id,country_name=area_name||'XT'
where
	flevel=1;

update base.dm_area 
set 
	country_id=-area_id,country_name=area_name||'XT'
where
	flevel=2;
```
![the-newest-area-info-01-05](http://7xl61k.com1.z0.glb.clouddn.com/the-newest-area-info-01-05.png-blog.photo)

好了，我们就处理好这个区域维度了，正常使用的话，估计就用flevel=3的就够了