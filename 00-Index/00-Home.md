---
cssclasses: dashboard
tags: [dashboard]
---

# 🚀 PPY's Control Center

> [!QUOTE] Daily Wisdom
> "The code you write makes you the programmer you are."

## 🎯 Focus & Capture
| **Action** | **Description** |
| :--- | :--- |
| [[01-Inbox\|⚡️ Capture]] | 快速捕捉想法、任务或未分类的笔记 |
| [[20-Areas/22-Life-Journal\|📅 Journal]] | 每日随笔、复盘与生活记录 |

---

## 🏗 Active Projects (10-Projects)
> [!SUMMARY] 🔥 最近活跃的项目
> *如果你安装了 Dataview 插件，下方会自动显示最近修改的 5 个项目文件*
> ```dataview
> TABLE without id file.link as "Project", file.mtime as "Last Modified"
> FROM "10-Projects"
> WHERE file.depth >= 2 and file.name != "0-MOC"
> SORT file.mtime desc
> LIMIT 5
> ```
> 
> **快速入口**：
> - [[10-Projects/11-毕业设计/0-MOC/项目总览|🎓 毕业设计 Dashboard]]

---

## 📚 Library & Learning (30-Resources)
> [!BOOK] 📖 在读列表
> ```dataview
> TABLE without id file.link as "书名", author as "作者"
> FROM "30-Resources/33-Reading/Books"
> WHERE contains(tags, "book/status/reading")
> ```

### 🧠 Knowledge Bases
- **CS Core**: [[408.base|408 核心数据库]] (你提到的 Database 视图)
- **Dev Wiki**: [[30-Resources/32-Dev-Wiki|技术文档索引]]
- **Reading**: [[30-Resources/33-Reading/00-Library-Dashboard|图书馆大厅]]

---

## 🛠 Areas & System (Maintenance)
- **💻 Dev Environment**: [[20-Areas/21-DevEnvironment|Neovim & Configs]]
- **📂 Archives**: [[40-Archives|归档记录]]
- **⚙️ System**: [[90-System/Templates|模板管理]] | [[90-System/Assets|附件管理]]

---
*Last Sync: `$= dv.current().file.mtime`*
