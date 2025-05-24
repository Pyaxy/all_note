---
tags: 
  - Questions
created: 2025-03-23 18:50
modified: 2025-03-23 18:50
---
# Django迁移依赖冲突

## 问题描述
- **现象**：`InconsistentMigrationHistory: Migration admin.0001_initial is applied before accounts.0001_initial`

## 根本原因
1. 直接原因：
   - **依赖顺序倒置**
	   - `django.contrib.admin` 的初始化迁移依赖 `auth` 应用，而 `auth` 应用又依赖你的自定义用户模型（`accounts` 应用）。但你的数据库历史中已存在 `admin` 的迁移记录，而 `accounts` 的迁移尚未应用。

## 解决方案
### **清理迁移历史**
```bash
# 进入docker容器
docker-compose exec web bash

# 删除所有迁移文件（容器内执行）
find . -path "*/migrations/*.py" -not -name "__init__.py" -delete
find . -path "*/migrations/*.pyc" -delete
# 删除数据库（或清空django_migrations表）
docker-compose down -v # 这会删除所有挂载的数据库卷
```

### 重新生成迁移
```bash
# 在容器中执行
python manage.py makemigrations
python manage.py migrate
```

### 验证方法
```python
# path: settings.py
# 必须放在所有第三方应用之前
AUTH_USER_MODEL = 'accounts.User' # 必须正确定义
INSTALLED_APPS = [ 'accounts.apps.AccountsConfig', # 你的自定义用户应用 
'django.contrib.admin', 
'django.contrib.auth', 
# ...其他应用 ]
```

**关联文档**：[[2025-03-23 开发日志]]