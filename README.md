# Fitness Journal

一个只给自己使用的 Android 健身记录工具。打开即用：记录身体、饮食、训练，查看趋势，并设置本地提醒。

**当前阶段：开发计划已建立，尚未创建 Android 工程，也没有可安装 App。**

## 从这里开始

1. AI 或开发者先读 [AGENTS.md](AGENTS.md)。
2. 打开 [接续入口](docs/START-HERE.md)，先识别公司/家用环境，再确定当前任务。
3. 查看 [完整开发路线](docs/DEVELOPMENT-PLAN.md) 与对应任务卡。
4. 每次完成一个任务，更新 [进度表](docs/progress/STATUS.md) 和会话记录。

## 已确认的方向

- 个人自用，先支持小米 Android；以后可能重做鸿蒙客户端。
- Kotlin + Jetpack Compose；Room 本地数据；本地计算和本地提醒。
- 三个主入口：总览、吃、练。设置从右上角进入。
- 主视觉：晴川蓝 #79BEDF 向缃云黄 #F4DC84 渐变，辅以冷暖对照。
- 不做账号系统、登录页面、自建服务端、订阅付费、社交或广告。
- 远程保存需求保留，具体网盘/协议暂不决定。第一版先实现可验证的文件导出与恢复，不能把它宣称为自动云备份。
- 代码仓库不存实际身体、饮食、用药数据、数据库、备份和签名密钥。

## 文档索引

| 文档 | 用途 |
| --- | --- |
| [产品范围](docs/PRODUCT.md) | 做什么、不做什么、页面和使用流程 |
| [开发路线](docs/DEVELOPMENT-PLAN.md) | 阶段、依赖、里程碑和任务清单 |
| [Windows 环境安装](docs/SETUP-WINDOWS.md) | 下载工具、JDK/SDK、真机连接、常见故障 |
| [架构](docs/ARCHITECTURE.md) | 单 App、本地数据流、代码目录、边界 |
| [视觉规范](docs/DESIGN-SYSTEM.md) | 渐变、字体、组件、图表、可访问性 |
| [数据模型](docs/DATA-MODEL.md) | 实体、历史快照、单位、删除和时间 |
| [计算规则](docs/CALCULATIONS.md) | 体重、能量、营养、训练比较和固定算例 |
| [备份格式](docs/BACKUP-FORMAT.md) | 可跨平台的数据文件与安全恢复 |
| [测试计划](docs/TEST-PLAN.md) | 测试类型、操作步骤、发布门槛 |
| [决策记录](docs/DECISIONS.md) | 已确认、暂定、明确推迟的事项 |
| [资料来源](docs/SOURCES.md) | 官方技术文档与计算依据 |
| [双环境索引](docs/progress/ENVIRONMENT.md) | 环境识别与本机准备检查 |
| [公司](docs/progress/environments/COMPANY.md) / [家用](docs/progress/environments/HOME.md) | 各自工具、网络与构建结果 |
| [跨电脑与同步手册](docs/MULTI-ENV-WORKFLOW.md) | 连接器成功经验、冲突处理与换机交接 |
| [进度状态](docs/progress/STATUS.md) | 已完成什么、下一步做什么 |

双环境规则补充：2026-11-03。项目进度共享，环境状态独立；公司已验证通道是GitHub连接器，不代表本地git push成功。

计划基准日：2026-11-02，北京时间。技术依赖在首次成功构建时锁定，不能把文档中的示例版本当成已验证组合。
