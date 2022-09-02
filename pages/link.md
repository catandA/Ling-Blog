---
layout: page
title: Links
tagline: My friends.
permalink: /links.html
---

---
layout: page
title: 友情链接
permalink: /friendlink/
tags: [友情链接]
feature-img: "assets/img/pexels/computer.jpeg"
---
## BGM-东方-Project:
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=500 height=150 src="//music.163.com/outchain/player?type=0&id=320880951&auto=1&height=430"></iframe>

---
### Tmaize
Tmaize的Blog--[**[Tmaize-Blog]**](https://blog.tmaize.net/)

---
### Cold Mint
Cold Mint--[**[铁锈助手2.0]**](https://www.coolapk.com/apk/com.coldmint.rust.pro)

---
### Mason
Mason的Blog--[**[Mason-Blog]**](https://mason369.github.io/Mason_blog/)

---
### Tianscar

碳酸天剑的Blog--[**[Tianscacr-Blog]**](https://blog.tianscar.com)  
碳酸天剑的网址导航--[**[Tianscacr-SiteNav]**](https://sitenav.tianscar.com)

---
{% include aligner.html images="feature-img/PTR3.png" %}
### Prohonor
Prohonor--[**[Progressive Tune]**](https://progressive-tune.github.io/ptr/)  

---
### Steveubuntu
Steveubuntu--[**[Steveubuntu]**](https://steveubuntu0.github.io/)

---
  
{% for f in site.data.friends %}
<div class="link-chip">
 <img alt="{{f.describe}}" src="{{f.image}}" class="link-chip-icon">
 <a title="{{f.describe}}" target="_ablank" class="link-chip-title" href="{{f.url}}">{{f.name}}</a>
</div>
{% endfor %}

[返回主页]({{ site.url }})

<hr/>

  {% if site.data.social.valine_comment.enable  == true %}
  <script src="/comment/av-min.js"></script>
  <script src="/comment/Valine.min.js"></script>
  <div id="comments"></div>
  {% include comments.html %}
  {% endif %}
  {% include scripts.html %}
