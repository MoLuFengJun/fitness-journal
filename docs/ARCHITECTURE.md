# 架构 v0.1

## 边界

一个 Android App，单个 app Gradle 模块，Kotlin + Jetpack Compose + Room。
没有自建后端、数据库服务器、登录系统或持续收费依赖。
参考 [官方离线优先指南](https://developer.android.com/topic/architecture/data-layer/offline-first)，本项目只实现本地优先与文件备份，不复制整套网络同步架构。

## 数据流

用户操作 → ViewModel → Repository → Room 事务 → Flow → UI。
计算：Repository 读取明确日期范围 → 纯 Kotlin 计算函数 → UI。
提醒：Room 配置 → ReminderScheduler → Android 系统；通知动作写回 Room。
备份：Room 一致性快照 → BackupCodec → 用户选定文件；恢复先校验再事务替换。

Room 是业务记录的唯一事实来源；不要让 UI 内存列表或图表缓存成为第二套真相。
页面当天合计即时根据本地记录更新，不依赖后台任务。
使用 Kotlin Coroutines/Flow；数据库/文件操作不在主线程。

## 建议目录（T02 创建，不代表现在已有）

~~~text
app/src/main/java/com/molufengjun/fitnessjournal/
  app/              Application、AppContainer、导航入口
  core/model/       单位、日期、领域模型
  core/calculation/ 纯计算与统计
  core/time/        可注入 Clock/ZoneId
  data/local/       Room entities、DAO、database、migrations
  data/repository/  按身体/饮食/训练/提醒分组
  feature/dashboard/
  feature/nutrition/
  feature/training/
  feature/settings/
  reminder/         scheduler、receiver、通知动作
  backup/           DTO、codec、validator、import/export
  ui/theme/         颜色、字型、形状、渐变
  ui/components/    公共数值卡、输入、空状态
app/src/test/       纯计算和序列化测试
app/src/androidTest/ DAO、迁移、UI、Android 文件/提醒测试
app/schemas/        Room schema，跟随 Git
~~~

应用 ID 暂定 com.molufengjun.fitnessjournal。T02 固定后，已有真实记录时不得随意更换。
debug 可使用 .debug 后缀，与长期自用 release 隔离，T26 通过文件迁移测试数据。

## 依赖选择

- Android Studio Stable 模板作为工具链起点；Gradle Wrapper 与版本目录入库。
- Compose BOM 统一 Compose 库版本；编译器插件版本与实际 Kotlin/AGP 配套。
- Room 与 KSP 在首次集成时查阅对应稳定版官方文档；不得混用 Room 2/3 的包名、插件和示例。
- ViewModel、Lifecycle、Coroutines、kotlinx.serialization。
- 手工 AppContainer 注入 Repository；当前不需要 Hilt/Koin。
- 图表优先用 Compose Canvas 实现有限图型，并提供文本摘要/数据列表；不造通用图表库。
- 当前不安装 Node、Fastify、PostgreSQL、Docker、Redis、Firebase。
- WorkManager 仅未来确有后台导出/上传任务时加入；它不是准点服药提醒计时器。

## 配置与数据

单例本地 Profile 与偏好可一并存 Room，减少配置和数据跨存储事务复杂度。
所有用户拥有的数据能导出；权限授权、系统通知 ID 和设备临时状态不跨设备恢复。
时间使用可注入 Clock，避免测试依赖真实今天。
长字符串/导入文件有大小限制；所有错误转换为明确中文提示并保留现有数据。

## 备份扩展

先实现 BackupCodec、BackupRepository 和通过系统文件选择器的文件读写。
不创建空壳云 SDK、占位账号、轮询服务或“已同步”图标。
未来 T29 根据选定网盘新增传输适配层；通用数据格式保持不变。
备份恢复是单设备快照替换，不等于多设备实时同步。

## 权限及设备能力

- 通知权限按需申请；拒绝不影响其他模块。
- 需要准确到点时再申请系统的提醒访问能力；拒绝时说明实际降级。
- 重启、时区/时间更改后按配置重新安排未来提醒。
- 系统文件选择器读写文件，不申请所有文件访问权限。
- 本地版本不申请网络权限，不添加分析/广告 SDK。
- 系统自动备份/设备转移行为在 T23 显式配置，不能因默认配置而承诺云端保存。
