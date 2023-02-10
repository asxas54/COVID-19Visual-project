# COVID-19Visual-project
# 牛津 COVID-19 政府响应追踪系统

数据源：https://github.com/OxCGRT/covid-policy-tracker

只看这一个数据covid-policy-tracker/data/China/OxCGRT_CHN_latest.csv

大意为中国各省的最新数据

一些数据说明：https://learn.microsoft.com/zh-cn/azure/open-datasets/dataset-oxford-covid-government-response-tracker?tabs=azure-storage

具体每个指标 covid-policy-tracker/documentation/ 下面的codebook.md和 index_methodology.md 等文档有说明

COVID-19Visual-project/images/下面有一些官方可视化图片

# 初步界面设计
![初步界面设计](https://github.com/asxas54/COVID-19Visual-project/blob/main/images/%E5%9B%BE%E7%89%877.png "初步界面设计")
## 全国地图 
确定一个时间节点，比如2022年6月1日 2022年12月1日，标注全国各省的以下指标
- StringencyIndex_Average 严格指数
- GovernmentResponseIndex_Average 政府响应指数
- ContainmentHealthIndex_Average 遏制和健康指数
- EconomicSupportIndex经济支持指数
先实现一个指标，再考虑不同指标的切换
再考虑不同日期
## 条形图
截止某个日期如2023年2月1日 
ConfirmedCases 确认感染人数
ConfirmedDeaths
如果显示不全可以是数量最多的最少的几个省
## 时间轴
2022年的时序数据 具体含义经过翻译
选几个城市 如北上广 重庆 绘制政策变动的时间轴 
C1M_School closing
1. 无措施
2. 建议关闭或所有学校开放，但与非Covid-19操作相比会产生显著差异
3. 要求关闭（仅部分级别或类别，如高中或公立学校）
4. 要求关闭所有级别
C2M_Workplace closing
1. 无措施
2. 建议关闭（或建议在家工作）或所有开业的企业，但与非Covid-19操作相比会产生显著差异
3. 要求某些行业或类别的工人关闭（或在家工作）
4. 要求除基本工作场所（如杂货店、医生）外的所有工作场所关闭（或在家工作）
时间轴参考效果

![时间轴参考效果](https://github.com/asxas54/COVID-19Visual-project/blob/main/images/%E5%9B%BE%E7%89%874.png "时间轴参考效果")

![时间轴参考效果](https://github.com/asxas54/COVID-19Visual-project/blob/main/images/%E5%9B%BE%E7%89%875.png "时间轴参考效果")

## 折线图
与时间轴图表 城市相对应
2022年该省的感染人数，可以以月为单位
ConfirmedCases 确认感染人数

## 饼图或环形图
- C1M_School closing 学校  
- C2M_Workplace closing 办公政策
- PopulationVaccinated 接种人数 有两种大于50% 小于50%
## 散点图
确诊人数和政府响应人数之间的散点图
参考官方图表

![时间轴参考效果](https://github.com/asxas54/COVID-19Visual-project/blob/main/images/%E5%9B%BE%E7%89%876.png  "时间轴参考效果")
# 参考页面

![时间轴参考效果](https://github.com/asxas54/COVID-19Visual-project/blob/main/images/%E5%9B%BE%E7%89%872.png  "时间轴参考效果")

![时间轴参考效果](https://github.com/asxas54/COVID-19Visual-project/blob/main/images/%E5%9B%BE%E7%89%873.png  "时间轴参考效果")
