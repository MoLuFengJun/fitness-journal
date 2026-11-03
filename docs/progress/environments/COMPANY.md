# COMPANY — 公司环境

## 已知事实（2026-11-02）

- 用户明确确认当时是公司电脑。
- 用户报告：公司网络等原因使git push经常失败。
- 本地工具观察：gh api user经代理连接失败，错误类别为proxyconnect / socket access forbidden。
- GitHub连接器对fitness-journal读写成功。**这不是本地git push成功。**
- 不能断言公司所有网络永久不可用，也不能将gh失败记为本轮实测git push失败。
- 当前文件是历史环境档案，不能代替新会话的环境识别。

## 工具与状态

| 项目 | 已观察 | 后续 |
| --- | --- | --- |
| OS/终端 | Windows / PowerShell | 核查必要差异 |
| Git | 可找到命令 | E01核验工作副本与基线 |
| Java | PATH存在，安装路径标识JDK23.0.1 | 核对与锁定AGP兼容的Gradle JDK |
| winget | 可找到 | 可选 |
| Node | 可找到 | 当前App不需要 |
| gh | 可找到，本地网络请求失败 | 当前不作为默认远程写入通道 |
| adb | PATH未发现 | 核查SDK真实位置，不等于完全未安装 |
| 全局Gradle | PATH未发现 | 正常，使用项目Wrapper |
| Studio / SDK | 常见位置未发现 | 未全盘扫描，不假称确定未安装 |
| 当前会话工作区 | 2026-11-02不是本仓库checkout | 远程写入不代表本机已clone |
| Android构建/手机运行 | 尚未执行 | E03/E04 |

## 通道策略与成功经验

当前无网络变化证据时，文档与小规模文本提交优先使用已提供、已授权的GitHub连接器。
不再循环尝试已知失败的gh路径；clone/fetch/push未实测，不应编造失败或成功。
具体步骤与冲突处理见 [同步手册](../../MULTI-ENV-WORKFLOW.md)。

已核实初始计划提交：1a17ab866f8c870ef4969907f3e59f9cb83803f1。
过程：Contents API初始化README/main → Git Data API建tree/commit → 非强制更新ref → 回读main与29个文件，内容一致。
该通道不上传本地.git历史，不证明本地Git传输、Gradle/SDK下载或Android构建可用。
新会话若无连接器，应说明能力缺失，不能凭历史宣称仍可写远程。

公开记录不包含公司代理地址、端口、用户名、机器名或凭据。

## 本机检查与版本

| 检查 | 状态 | 证据 |
| --- | --- | --- |
| E00 | CONFIRMED_ON_2026-11-02 | 用户当日确认，不自动沿用到未来 |
| E01 checkout | NOT_VERIFIED | 前述会话目录不是checkout |
| E02 Android工具 | PARTIAL | 仅初步盘点 |
| E03 构建 | NOT_RUN | |
| E04 手机 | NOT_RUN | |

| 项目 | 实际值 | 验证日期/提交 |
| --- | --- | --- |
| Studio Stable | 待填 | |
| AGP / Wrapper | 以仓库锁定值为准，首次待定 | |
| Gradle JDK/JBR | 待填 | |
| Kotlin / Compose compiler / BOM | 待核验 | |
| KSP / Room系列 | 待集成 | |
| compileSdk / targetSdk / minSdk | 待填 | |
| applicationId / debug suffix | 待填 | |
| 手机型号 / Android / HyperOS | 待填，不填序列号 | |
| 最后构建SHA | 无 | |
| 最后本地Git传输成功 | 未验证 | |
| 连接器成功提交 | 上述计划提交；后续见Git历史/会话记录 | |

本机通过才填写本机PASS，不把另一台的结果复制过来。
