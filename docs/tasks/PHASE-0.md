# 阶段0：工具和首次运行

共同要求：完成一项就更新STATUS和会话记录。当前没有工程，不执行不存在的Gradle命令。

## T00 获取仓库与核对范围

输入：AGENTS、START-HERE、PRODUCT、DECISIONS、SETUP-WINDOWS。
操作：
1. 确认当前路径与git remote；若未克隆，用SETUP第0节获取仓库。
2. 检查工作区改动和分支，保护已有文件。
3. 读已确认范围；把单人、本地优先、无登录、远程待定写入本轮说明。
4. 从main创建task/T00-workspace；已有该分支则先核对，不覆盖。
5. 检查.gitignore能覆盖数据库/备份/签名文件，同时不忽略Wrapper JAR与Room schema。
产出：正确工作副本、环境与任务状态更新；不生成业务代码。
验收：git status和remote符合目标仓库；文档可读；不含私人记录。
停止：网络阻塞则明确记录，不能说已经clone；连接器只可作已授权的远程文件通道。
下一项：T01。

## T01 安装并验证开发工具

输入：SETUP-WINDOWS、ENVIRONMENT、官方A01/A02。
操作：
1. 盘点Git、Studio、SDK、JDK；记录实际存在情况。
2. 仅缺失时下载Git和Android Studio Stable。
3. 首次启动完成SDK Platform、Platform-Tools及必要Build-Tools安装。
4. 对齐Studio与终端Gradle JDK；不破坏系统其他Java项目。
5. 决定真机优先，模拟器可选；不安装服务端工具。
产出：ENVIRONMENT填写安装位置的通用说明、实际版本、未完成项。
验收：Studio可启动；SDK Manager显示所需包已安装；java -version指向拟用JDK；adb可用完整路径执行。
停止：系统权限、网络或安装需用户操作时，给出具体一步，记录BLOCKED。不要使用未知镜像或关闭TLS验证。
下一项：T02。

## T02 创建最小Android工程并锁定版本

输入：ARCHITECTURE、SETUP第5节、ENVIRONMENT。
操作：
1. 用稳定Studio的Empty Activity Compose模板生成工程。
2. 固定包名/目录；模板生成到临时目录时仅移入工程文件，不覆盖现有文档。
3. 按模板兼容关系锁定AGP、Wrapper、Kotlin、Compose compiler、BOM；使用libs.versions.toml。
4. 配置debug身份标识，长期release包名保持稳定。
5. 保留一个最小页面，只显示产品名；不加入数据库/图表/提醒。
6. 记录JDK与SDK版本，不提交local.properties。
产出：可构建的app工程和版本记录。
验收：
~~~powershell
.\gradlew.bat --version
.\gradlew.bat :app:assembleDebug
.\gradlew.bat :app:testDebugUnitTest
.\gradlew.bat :app:lintDebug
~~~
确认Wrapper相关文件纳入Git。NO-SOURCE不得写业务测试通过。
停止：兼容错误先按官方说明修复，不能无依据轮换版本。
下一项：T03。

## T03 小米首次安装运行

输入：SETUP第6节、TEST-PLAN、已构建APK。
操作：
1. USB数据线、开发者选项、授权ADB，核对设备列表。
2. 记录型号/Android/HyperOS版本，不记录序列号。
3. Android Studio Run安装debug包，打开、退后台、再次进入。
4. 保存一张不含私人信息的运行截图或简短人工检查记录。
产出：首次真机证据，ENVIRONMENT更新。
验收：设备显示device，App启动无崩溃，显示预期页面。
停止：无手机可用标NEEDS_DEVICE_CHECK；可继续T04，不得标真机通过。
下一项：T04。
