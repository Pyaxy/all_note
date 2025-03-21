---
tags:
  - Questions
date: 2025-03-18
---
## 问题1：`exec /entrypoint.sh: permission denied`
- 原因：宿主机脚本权限不够
- 解决方案
	1. 检查`Dockerfile`是否修改脚本权限，如果没有，则加上`RUN chmod +x /entrypoint.sh`，之后重新build镜像。
## 问题2：加入`RUN chmod +x /entrypoint.sh`后仍然显示权限不足
- 原因：在Dockerfile中复制脚本并修改权限后，如果在后续使用volume将脚本可持续化到容器中，宿主机的原始脚本会将修改权限后的脚本覆盖
- 解决方案
	1. 在宿主机中运行`chmod +x ./entrypoint.sh`，将脚本权限修改之后再复制到容器中。