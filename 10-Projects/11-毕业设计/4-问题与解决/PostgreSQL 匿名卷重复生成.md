---
tags: 
  - Questions
created: 2025-03-21 21:14
---
# PostgreSQL 匿名卷重复生成

## 问题描述
- **现象**：
	- `docker-compose up` 时自动创建匿名卷

## 根本原因
1. 直接原因：
	- PostgreSQL 卷路径拼写错误
## 解决方案
### 永久方案
```diff
# project/docker-compose.yml#L21
- postgres_data:/var/lib/postgres/data
+ postgres_data:/var/lib/postgresql/data

```
**关联文档**：[[2025-03-21 开发日志#❗ 遇到的问题]]