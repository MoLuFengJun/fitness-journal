# 环境记录

## 2026-11-02：只读初步盘点

本记录只描述计划制定时的检查，不表示Android环境就绪。
公开仓库不保存机器用户名、完整个人路径、设备序列号或代理凭据。

| 项目 | 观察 | 状态 |
| --- | --- | --- |
| Git | 可找到命令 | 版本与克隆需T00/T01核验 |
| Java | PATH可找到，安装路径标识JDK 23.0.1 | 未验证适配AGP；不能默认用于Android |
| winget | 可找到命令 | 可选，不必使用 |
| Node | 可找到命令 | 本项目无需使用 |
| GitHub CLI | 可找到命令；此前API请求受当前会话网络/代理限制 | 不等于用户电脑所有网络不可用 |
| adb | PATH未找到 | 不等于磁盘绝无SDK |
| 全局Gradle | PATH未找到 | 正常；工程应使用Wrapper |
| Android Studio | 常见Program Files位置未发现 | 未全面扫描，安装状态待核验 |
| Android SDK | 常见用户Local AppData位置未发现 | 实际路径待核验 |
| 真机 | 未连接检查 | 型号/系统/安装均待T03 |
| 远程仓库 | 已存在、默认分支main、初始为空、可见性公开 | 本轮只建立计划文档 |

## 首次成功构建后填写

| 项目 | 实际值 | 证据/验证日期 |
| --- | --- | --- |
| Android Studio Stable | 待填 | |
| AGP | 待填 | |
| Gradle Wrapper | 待填 | |
| Gradle JDK / JBR | 待填 | |
| Kotlin（实际生效） | 待填 | |
| Compose compiler | 待填 | |
| Compose BOM | 待填 | |
| KSP | T07填写 | |
| Room及包名系列 | T07填写 | |
| compileSdk / targetSdk / minSdk | 待填 | |
| applicationId / debug suffix | 待填 | |
| 小米型号 / Android / HyperOS | 待填 | 不含序列号 |
| 首次assembleDebug | NOT_RUN | |
| 首次安装启动 | NOT_RUN | |

默认依赖文件记录精确版本；这里用于解释与核查，不创建第二套版本真相。
