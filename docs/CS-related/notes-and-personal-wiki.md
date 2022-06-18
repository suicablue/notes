---
title: Foam、Obsidian、Hugo、Mkdocs————笔记写作与发布探索之旅
date created: 2022-01-11 03:20
status: post, draft
tags:
  - null
aliases: Foam、Obsidian、Hugo、Mkdocs————笔记写作与发布探索之旅
date updated: 2022-01-11 03:23
---

# Foam、Obsidian、Hugo、Mkdocs————笔记写作与发布探索之旅
*可以将这篇看作 portfolio notes-and-posts ，也就是[[multiple-vaults-managment|筆記、博文與多項目管理]]这篇结论性质的文的思路叙述。*

## 从 Obsidian 到 Foam+MKDocs

- 多出了发布「相对于博文状态不完善的」笔记的需求
- 对于多设备使用需求产生质疑

### 具体说说需求变化

从今年年中开始因 TL 友邻推荐，我开始在电脑与手机上同时使用 Obsidian 及其付费 sync 功能。起初 Obsidian 的使用感很让人舒服，无论外观还是功能（包括主体和丰富的插件）都令人满意，这期间我曾写过一篇 Obsidian 使用相关的笔记（ [如何利用 obsidian 高效建立笔记](https://suicablog.cobaltkiss.blue/notes/CS-related/note%26diary-related/Obsidian-notes/) ）。尽管我会时不时抱着不想花钱的心态研究非官方sync的多设备同步方案，但最后因为既不想用非端对端的不安全加密方式而拒绝了第三方云盘方案，又不想在手机上放密钥来实现手机git操作（万一手机被偷了呢），最后干脆还是老实花钱。

与此同时，自 Obsidian 使用以来我写了不少笔记，数量远比发布在本博客的文要多，不由得让人想要把东西摆在页面上，就算没人看自己浏览着页面也会有满足感。我很希望笔记页面和博客页面是风格一致的，试图寻找一个直接将 Obsidian 上的笔记转换到 hugo 博客上的方案，期间遇到许多麻烦：

- Obsidian 的特殊语法无法在 hugo 博客页面上正常显示
- 我目前的 Hugo 主题不像许多主题有原生的多板块功能，需要手动设计配置（我也尝试了许多其他主题，但还是对目前的主题最满意）
- 要想笔记在 hugo 上正常显示，我得一篇篇修改 metadata ，很麻烦
- 多文档下的笔记不方便在当前 Hugo 主题下显示

最近国内流行的 Markdown 编辑器 Typora 推出付费服务后被墙，许多人在寻找替代应用时开始使用 Obsidian。正巧看到一位友邻使用后分享了免费的多设备同步方案，此时我已有一阵子没有使用 Obsidian，于是开始反思我的笔记记录需求：我真的有在手机上编辑笔记的需求吗？我一边思考一边在网上胡乱搜索，恰好看到了 [这个让笔记在网页上显示的工具](https://github.com/Jackiexiao/foam-mkdocs-template) ，十分适合做个人wiki或笔记展示， [funkwhale的文档](https://docs.funkwhale.audio/index.html) 似乎也是用了MKDocs的一个主题制作的。

[[MKDocs-notes-installation-and-deployment|MKDocs 的安装与部署（搭配 Obsidian ）]]

### 在实际体验中发现需求再次产生变化
- vs code 对于「纯写作」并不如想象中友好
- 多设备同步依旧是必要的


## 从 Foam 回归 Obsidian

[[Obsidian-notes|如何利用 obsidian 高效建立笔记]]

### syncthing 同步方案

### Obsidian 插件提升编辑与管理体验

### 编辑时与 Hugo 格式 metadata 冲突，多 vault 管理想法冒出

### submodule & subtree 同一篇文，即是笔记又是博文

### 笔记堆积影响写作体验，可我无法割舍双向链接


## MKdocs 还是 Hugo

習慣了發 MKdocs 後我感覺我發文到主博客的自我要求更高了，結果就是更不想發博文了（筆記不是博文），但是記亂七八糟的筆記頻率變高了，總體來說寫東西的頻率變高了。  
  
昨晚大腦暴走想了一堆筆記啊博文啊本地記錄和遠程發佈啊相關的東西，感覺現階段還是以積累筆記、回顧整理筆記爲主，等到筆記通過雙鏈得到的 graph 圖明顯呈現出一團一團的樣子、少有零散的筆記後再分別部署到不同個人 wiki 或博客上，在此之前沒必要再部署、發佈和主題選擇上費太多功夫。  
  
wiki-links、標題、文檔名稱、slug 等在 Obsidian 與其他靜態網站上的衝突是個看似不太重要但仔細研究下就會覺得很麻煩的東西，昨天大概想到一套方案但還需實際檢驗，不過這些都是筆記整理到一定程度後才需要主要考慮的事了……

### 多样化主题外观需求

### 老问题， wiki-links 在 Hugo 上的实现效果

### 阉割 wiki-links 与 Obsidian 语法在多个项目和 vault 上的共存
- vault 根目錄與博客根目錄保持一致。
- 使用 MOC 結構替代文件夾結構兼容大部分靜態博客結構
- 給每個 Hugo 博客都加上 wiki-links 轉換效果
    - 分離出新的主題 vault 前重新整理這個 vault 下所有筆記
    - 將另文件名與標題相同（Obsidian 雙鏈標識是文件名而 Hugo 那個是 title）
    - 因為標題都是中文，所以網頁鏈接會變得很醜，但因為它們已經單獨在另一博客發布了，所以儘管本地筆記 vault 還保留原筆記，他們可以不再在筆記發布頁面發布了（看不到就沒有不優雅的鏈接）

## 近期的最终方案（讲解图）

脑袋里、键盘上折腾来折腾去，就是想要个清晰明了的大脑。我们看到太多清晰明了的优雅结论，好羡慕，可少有人分享这个结论是如何一点点诞生的，只好自己慢慢在思考与实践中摸索并记录。人们说内容大于方法，但探索方法的过程对我而言既重要又是有趣的。愿这些或清晰或凌乱的记录能帮到也在纠结类似的事情的你。