# 资料来源与核验

核查日期：2026-11-02。优先使用原始研究和官方文档。
这些链接是核验入口，不表示本仓库已经安装或测试了其中所有最新版本。
实施时出现版本冲突，记录当前稳定版本与兼容关系，不把网页示例自动复制为全项目版本。

## Android

- A01 Android Studio下载：https://developer.android.com/studio
- A02 Windows安装：https://developer.android.com/studio/install
- A03 AGP 9内置Kotlin与迁移：https://developer.android.com/build/releases/agp-9-0-0-release-notes
- A04 Compose编译器配置：https://developer.android.com/develop/ui/compose/setup-compose-dependencies-and-compiler
- A05 Compose BOM：https://developer.android.com/develop/ui/compose/bom
- A06 自定义设计系统：https://developer.android.com/develop/ui/compose/designsystems/custom
- A07 渐变绘制：https://developer.android.com/develop/ui/compose/graphics/draw/brush
- A08 本地优先架构：https://developer.android.com/topic/architecture/data-layer/offline-first
- A09 Room：https://developer.android.com/training/data-storage/room
- A10 数据库迁移：https://developer.android.com/training/data-storage/room/migrating-db-versions
- A11 数据库测试：https://developer.android.com/training/data-storage/room/testing-db
- A12 真机运行：https://developer.android.com/studio/run/device
- A13 提醒与权限：https://developer.android.com/develop/background-work/services/alarms
- A14 通知权限：https://developer.android.com/develop/ui/compose/notifications/notification-permission
- A15 系统文件选择器：https://developer.android.com/training/data-storage/shared/documents-files
- A16 应用签名：https://developer.android.com/studio/publish/app-signing
- A17 Git Windows下载：https://git-scm.com/downloads/win

## 计算来源

- S01 Mifflin等，1990，静息能耗预测研究：https://pubmed.ncbi.nlm.nih.gov/2305711/
  本计划采用常用简化公式并显式标版本；估算不等于测量。
- S02 Morton等，2018，抗阻训练蛋白补充系统综述与Meta分析：https://pubmed.ncbi.nlm.nih.gov/28698222/
  群体研究结果不能当作某个人的精确需要、减脂期最优量或医学上限。
- S03 2024 Adult Compendium：https://pacompendium.com/
  活动表：https://pacompendium.com/wp-content/uploads/2024/01/2024-adult-compendium-tracking-guide_1-2024.pdf
  MET用于粗估，不证明某次力量训练的实际消耗。
- S04 NIDDK体重规划工具：https://www.niddk.nih.gov/bwp
  用于理解活动与能量目标估计；本App不复制其完整动态模型。

## 工程推断与产品规则

以下是本产品的设计决定，不伪装成官方/医学结论：
- 单模块、无后端、手工依赖注入、Canvas有限图表。
- 10%缺口快捷选项、用户确认的宏量参数、低于静息估算的检查提示。
- 日记完整标记、每日最后一次测量、7/30天窗口、同条件训练比较。
- 先文件备份再决定远程供应商。
- 版本锁定后避免无关升级。

官方文档证明技术能力，不证明本App已正确实现；实现结果以构建、测试和实机为准。
