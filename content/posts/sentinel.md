---
title: "Business data driven dynamic flowRules in Sentinel"
date: 2022-12-12T21:58:11+08:00
draft: false
---
## Native dynamic datasource supported by Sentinel
In fact, Sentinel already build-in support dynamic datasource 👇
* [dynamic rules](https://github.com/alibaba/Sentinel/wiki/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%99%E6%89%A9%E5%B1%95)
* [using in product environment-sentinel](https://github.com/alibaba/Sentinel/wiki/%E5%9C%A8%E7%94%9F%E4%BA%A7%E7%8E%AF%E5%A2%83%E4%B8%AD%E4%BD%BF%E7%94%A8-Sentinel#%E8%A7%84%E5%88%99%E7%AE%A1%E7%90%86%E5%8F%8A%E6%8E%A8%E9%80%81)

Sentinel official suggested that the dynamic rules pushed by sentinel-datasource rather than sentinel-client, 
like the following 👇
![](https://raw.githubusercontent.com/Yan1025/picbed/master/picbed/53381986-a0b73f00-39ad-11e9-90cf-b49158ae4b6f.png)
Howeve my demand is making the flowRules change according to the business data rather than my operation on the sentinel-dashboard.

## Transform the interaction between systems
So we need transform the interaction between sentinel-client, etcd server and sentinet-dashboard, like the following
(i used the etcd as the dynamic datasource)👇
![](https://raw.githubusercontent.com/Yan1025/picbed/master/picbed/%E6%9C%AA%E5%91%BD%E5%90%8D%E6%96%87%E4%BB%B6%20(90).png)

Besides the common steps between offical suggested and my practice, we should takes one more step: 
add a listener to the business data(a collection named 'Sla' that stored in MongoDB) what builds new flowRules from the 
new business data and pushs to etcd server positively. Sentinel-etcd-datasource will automatically does the rest things.

## The bug of sentinel
I found a bug when i used sentinel-etcd-datasource, and i committed a pull request which is already merged into the 
master branch and will be released in about v2.0.0👇
![](https://raw.githubusercontent.com/Yan1025/picbed/master/picbed/20221224.png)