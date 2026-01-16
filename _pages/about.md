---
permalink: /
title: "About Me"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

<!-- ===== 切换按钮 ===== -->
<div style="margin-bottom:16px;">
  <button onclick="showTab('intro')">个人介绍</button>
  <button onclick="showTab('report')">数据报告</button>
</div>

<!-- ===== 个人介绍 ===== -->
<div id="intro">

Hi, I’m Wei Chen. I did my undergraduate studies at the School of Health Humanities, Peking University, and I’m now a master’s student at Peking University Third Hospital.

Academically, I’m what people kindly call a “free-range researcher”—I currently have exactly one manuscript under review, which I cherish like an only child. On the bright side, I did pass TEM-8, so at least my English is more reliable than my publication record.

Outside the lab, I’m the captain of the Peking University Bodybuilding Team, where I lift both weights and team morale. I’ve competed a bit too:

- Overall Champion in Fitness Model, Peking University’s First Fitness & Bodybuilding Championships  
- 1st place, Fitness Model (Group B), 14th Beijing Sport University Fitness & Bodybuilding Championships  
- 3rd place, Men’s Physique (Group E), 19th Capital University Fitness & Bodybuilding Championships  
- 4th place, Men’s Physique (Group E), 20th Capital University Fitness & Bodybuilding Championships  

In short: mild academic chaos, strong English, and even stronger delts.

</div>

<!-- ===== 数据报告 ===== -->
<div id="report" style="display:none;">

## 数据报告展示

<p>
  <a href="/files/report.pdf" target="_blank">打开 / 下载 PDF</a>
</p>

<iframe
  src="/files/report.pdf"
  width="100%"
  height="800"
  style="border:1px solid #ddd;">
</iframe>

</div>

<script>
function showTab(tab) {
  document.getElementById('intro').style.display =
    tab === 'intro' ? 'block' : 'none';
  document.getElementById('report').style.display =
    tab === 'report' ? 'block' : 'none';
}
</script>
