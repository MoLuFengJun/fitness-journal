# HOME — 家用环境

## 当前认识

用户准备在家用电脑接续开发，表示工具可能还没搭建。
当前没有家用机器检测结果，所有能力待核实；UNKNOWN不是确认未安装。
不继承公司JDK路径、网络限制、代理或签名文件。

## 首次到家后的步骤

1. 按 [识别规则](../ENVIRONMENT.md) 确认HOME，填写本机标记。
2. 读main最新STATUS、上一交接，确定接续的远程分支/SHA。
3. 检查已有checkout与未提交工作；没有正确checkout再clone。
4. 按 [安装指南](../../SETUP-WINDOWS.md) 盘点缺项，已有可用工具不重复安装。
5. 仓库已有Android工程时，直接导入，使用已提交Wrapper/版本目录；不要再次New Project覆盖工程。
6. 安装匹配SDK/JDK，在指定提交上构建，记本机E03。
7. 手机可用时验证E04；安装前核对包名和签名，不误卸载已有数据。
8. 从共享STATUS继续项目任务，环境补齐不重置全局进度。

## 能力与版本

| 检查 | 状态 | 日期/证据 |
| --- | --- | --- |
| E00 归属确认 | UNKNOWN | 等用户本轮确认或匹配的本机标记 |
| E01 checkout/基线 | UNKNOWN | |
| E02 安装与兼容 | UNKNOWN | 先盘点 |
| E03 构建 | NOT_RUN | |
| E04 手机 | NOT_RUN | |
| 本地Git clone/fetch/push | UNKNOWN | 不继承公司失败 |
| GitHub连接器 | UNKNOWN | 检查当前会话工具 |

| 项目 | 实际值 | 验证日期/提交 |
| --- | --- | --- |
| OS / shell | 待确认；若非Windows，按本机系统调整安装步骤 | |
| Git | 待填 | |
| Studio Stable | 待填 | |
| AGP / Wrapper | 匹配仓库版本 | |
| Gradle JDK/JBR | 待填 | |
| Kotlin / Compose compiler / BOM | 待核验 | |
| KSP / Room系列 | 待核验 | |
| compileSdk / targetSdk / minSdk | 待填 | |
| applicationId / debug suffix | 匹配仓库 | |
| 手机型号 / Android / HyperOS | 待填，不填序列号 | |
| 最后构建SHA | 无 | |
| 最后成功传输通道 | 未验证 | |

普通Git网络正常时优先使用普通Git；有失败后按 [同步手册](../../MULTI-ENV-WORKFLOW.md) 处理。
只记录脱敏结果，不上传家用网络、机器名、私人路径或密钥。
