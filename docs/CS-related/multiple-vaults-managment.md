---
title: 筆記、博文與多項目管理
date created: 2022-01-17 07:42
status: note, draft
tags:
  - note
  - vault
  - post
  - blog
aliases: 筆記、博文與多項目管理
---
# 筆記、博文與多項目管理

*思路整理： [[notes-and-personal-wiki|笔记写作与发布探索之旅]]*

## 本地與遠程、筆記與博文

### 本地筆記編輯與管理
1. Obsidian 編輯器兼格式管理

註： 利用 Graph 功能建立筆記間關連、分類筆記群，已發布的筆記不建議刪除，但可以不再繼續在筆記 vault 中更改，轉換為由筆記 vault 拉取其他 vault 的變動。

### 筆記發布（過渡）
1. MKdocs 發布筆記到 suicablog.cobaltkiss.blue/notes （格式一致）

註： 若主題個人 wiki 已完成大部份內容整理（在 Graph 上表現為一大片筆記明顯聚合在一起且於其他筆記沒有明顯關聯），便可不再繼續在筆記 vault 中進行修改，改為在另一 vault 中修改後由筆記 vault 拉取，刪除筆記 vault 中發布的這一部分 section ，並單獨建立同步文檔與手機同步，確認在特定博客中發布的草稿狀態博文不必存放在筆記 vault 中，不確定存放位置或發布與否的文暫存與筆記 vault ，未來發布後可從中刪除

註： 跨 vault 的 subtree 拉取後，若其他 vault 使用絕對路徑，筆記 vault 由於結構不同將無法顯示雙鏈，應在 vault 靜態博客上使用根據 title 進行識別的語法轉換器，保持文檔名和 title 一致並使用 slug 來優雅顯示網址。即便筆記 vault 頁面因無法像 Hugo 一樣識別 slug 的 metadata 且中文網址不優雅也無妨，因為已分離的 section 內的筆記將在之後不再繼續發布

### 在本地將筆記轉化為博文（過渡）
1. Obsidian 內編輯（如 mixing）
2. 其他編輯器編輯 （如 Hugo 博客部分文件）

註： Obsidian 統一調整格式，注意部分 metadata 也會隨之變更為其他靜態頁面無法識別的樣式（如 Hugo 的封面 metadata 被修改後出現報錯，需要手動調回原狀）

更新： 是不是新建一個vault，裡面按照類型放發布的博文和未發佈計畫中或撰寫中的草稿比較好？不要把筆記 vault 當草稿和筆記存放地了？
這樣可以不再將注意力放在倉庫同步於 wiki-links 這些內容與結構無無關的事情上。

### 遠程發布博文
1. suicablog.cobaltkiss.blue 以此博客為中心向外擴散其他項目，並存放各種各樣不限主題的博文
2. blog.moonblast.boats/mixing 調酒文章發布地 （類似的，其他分類明顯的項目都這樣發布， blog.moonblast.boats 為未來個人主頁，主域名為單靜態頁面不介紹任何其他項目）
3. （待做）docs.cobaltkiss.blue 實例相關文檔存放地

思考： 已發布的博文是否有必要在筆記中保留？已自成一個 vault 的文檔集是否有必要留在筆記 vault 中，若去除，是否需要建立更多同步文件夾

註： 發布的目的是分享，GitHub 即是媒介也是備份工具，發布過程中遇到語法格式問題（如雙鏈不兼容）應單獨製作 vault 不使用特殊語法，在依舊可以使用 Graph 和雙鏈分析的前提下發布博文

註： 區分個人 wiki 和博客，前者可以展示草稿狀態的文章，而後者盡量只展示完成狀態的博文，草稿狀態的文為 true 的 draft 狀態。

### SNS status 管理
1. Obsidian 時間軸插件按照時間順序整理精華 status
2. （可選）Hugo 時間線主題展示部分整理後 status
3. （待檢驗）利用網頁插件實時得到 tag 下 status ，複製全部得到一個文檔，之後利用 Obsidian notes 插件將其快速分割為多篇日記
4. （待檢驗）IFTTT rss to Google Drive 測試長毛象與 Pleroma ，查看實際收到的 status 是什麼格式，是否可以導入 Keep 中快速聚合