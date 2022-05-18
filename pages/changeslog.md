---
layout: page
title: 维护日志
permalink: /changes/
tags: [ChangesLogs,Blog,Jekyll]
---

---
## 2022-5-18——(博客维护日志-类型:优化)
1.将Cusdis评论系统加入,并加入中文语言包系统  
2.优化博客加载布局，加载时间更简短  
3.对TOS主题进行部分区域优化，主题调整为夜间并无法调整为正常

---
## 2022-5-17——(博客维护日志-类型:建站)
1.基于[Jekyll](https://jekyllrb.com/)引擎构建博客  
2.应用[Type-On-Strap](https://github.com/sylhare/Type-on-Strap)主题  
3.加载图像压缩优化,<font color="#ff00ff">SCSS</font>布局优化调整

---
<div>
<p style="text-align:center;padding-top:5px;">Loading……:<span id="days">0</span>天
</p>
</div>
<script>
var s1 = '2022-05-17';//设置为你的建站时间
s1 = new Date(s1.replace(/-/g, "/"));
s2 = new Date();
var days = s2.getTime() - s1.getTime();
days =parseInt( days / (1000 * 60 * 60 * 24));
var number_of_years = parseInt(days/365);
days%=365;
var number_of_months = parseInt(days/30);
days%=30;
var number_of_days = parseInt(days);
var timeHTML="绫中之书已经安静的运行了";
if(number_of_years)
    timeHTML+=number_of_years+"年";
if(number_of_months)
    timeHTML+=number_of_months+"月";
if(number_of_days)
    timeHTML+=number_of_days+"天。";
document.getElementById('times').innerHTML = timeHTML;
</script>