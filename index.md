---
layout: default
title: "我的賽博龐克基地"
---

# 歡迎來到我的 AI 專案部落格！

這裡正在使用 **Hacker Theme** 賽博龐克風格建設中...

---

## 📅 技術筆記歷程 (按天數排序)

<ul>
  {% for post in site.posts %}
    <li style="margin-bottom: 15px;">
      <span style="color: #00ff00; font-family: monospace; font-weight: bold;">[{{ post.date | date: "%Y-%m-%d" }}]</span> — 
      <a href="{{ post.url }}" style="color: #00ffff; text-decoration: none; font-weight: bold;">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
