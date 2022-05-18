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

<p style="text-align:center;padding-top:5px;">绫中之书已在这个世界运行：<span id="days">0</span>天
</p>
<script>
	var s1 = '2022-05-17'
	s1 = new Date(s1.replace(/-/g, "/"));
	s2 = new Date();
	var days = s2.getTime() - s1.getTime();
	var number_of_days = parseInt(days / (1000 * 60 * 60 * 24));
	document.getElementById('days').innerHTML = number_of_days;
</script>