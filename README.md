# 张老师物理工作台 · 测评与恢复更新

适用于已有「张老师物理」本机工作台中已包含按题聚焦校对、题型总览多选和逐生订正的版本。此仓库保存本批更新的代码补丁，适用于具备现有本机部署的环境。

## 内容

- 四个 SQLite 库与原卷、录课和交付成品同批备份。恢复验证文件 SHA-256 和数据库完整性，将生产记录中的归档路径迁入新目录。
- 单选题支持按确认过的同一答项选择学生、预览、应用和撤销。
- 订正通过后使用不同试卷的已核验同考法题做独立复测。首答留存，单选依审核答案判定，其余交教师复核。
- 补丁包含可重复运行的单元测试。已有本机程序、资料库和模型配置由运行环境提供。

## 安装更新

在工作台源码根目录用 Node.js 25 运行。先备份程序和现有数据，在本机副本完成验证。

```sh
git apply --check /path/to/workbench-assessment-recovery.patch
git apply /path/to/workbench-assessment-recovery.patch
npm run build
npm test
```

复验通过后，再按本机部署方式更新并重启服务。

## 创建和试恢复备份

```sh
node scripts/workbench-recovery.mjs snapshot /path/to/live-data /path/to/feedback /path/to/new-backup
node scripts/workbench-recovery.mjs restore /path/to/new-backup /path/to/new-restore
PORT=4191 PANEL_RECOVERY_READ_ONLY=1 PANEL_DATA_DIR=/path/to/new-restore/data PANEL_FEEDBACK_DIR=/path/to/new-restore/feedback node local-server.mjs
```

输出目录须尚未存在。恢复成功生成 `restore-ready.json`，预览服务受本机回环和只读限制。备份环境按内部教学资料管理。
