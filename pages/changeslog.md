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
 <div class="ui inverted section divider"></div>
            <p>
                <span id="timeDate">载入天数...</span><span id="times">载入时分秒...</span>
            </p>
            </div>
<script>
    var now = new Date();

    function createtime() {
        var grt = new Date("18/05/2022 00:00:00");
        now.setTime(now.getTime() + 250);
        days = (now - grt) / 1000 / 60 / 60 / 24;
        dnum = Math.floor(days);
        hours = (now - grt) / 1000 / 60 / 60 - (24 * dnum);
        hnum = Math.floor(hours);
        if (String(hnum).length == 1) {
            hnum = "0" + hnum;
        }
        minutes = (now - grt) / 1000 / 60 - (24 * 60 * dnum) - (60 * hnum);
        mnum = Math.floor(minutes);
        if (String(mnum).length == 1) {
            mnum = "0" + mnum;
        }
        seconds = (now - grt) / 1000 - (24 * 60 * 60 * dnum) - (60 * 60 * hnum) - (60 * mnum);
        snum = Math.round(seconds);
        if (String(snum).length == 1) {
            snum = "0" + snum;
        }
        document.getElementById("timeDate").innerHTML = "本Blog已经运行了 " + dnum + " 天 ";
        document.getElementById("times").innerHTML = hnum + " 小时 " + mnum + " 分 " + snum + " 秒";
    }

    setInterval("createtime()", 500);
</script>
