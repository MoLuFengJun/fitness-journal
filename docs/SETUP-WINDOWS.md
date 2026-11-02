# Windows 开发环境：按顺序操作

对应 T00–T03。本文件是后续执行说明，本轮没有安装软件。
先记录当前环境，已存在并可用的工具不重复安装。

## 0. 获取仓库（T00）

推荐使用短、无空格的目录，例如 D:\workspace\fitness-journal；当前会话工作区并不等于已克隆的仓库。
先检查目标目录是否存在及是否有文件。已有工作目录时先检查 git remote，不在已有项目里再次初始化。

新目录首次获取：
~~~powershell
git clone https://github.com/MoLuFengJun/fitness-journal.git D:\workspace\fitness-journal
Set-Location -LiteralPath 'D:\workspace\fitness-journal'
git status --short --branch
git remote -v
~~~

读取 AGENTS.md、START-HERE、STATUS，再创建当前任务分支。
如果网络失败，记录网络/代理错误；不要反复重建仓库或关闭证书验证。
GitHub 连接器能访问而本地 Git 不能访问时，明确它们是不同通道，不能把远程 API 成功写成克隆成功。

## 1. 盘点（T01）

~~~powershell
git --version
Get-Command java,adb,winget -ErrorAction SilentlyContinue
~~~

在 ENVIRONMENT 中记录检测结果。java 命令存在不代表它适合当前 Android Gradle 组合。
不打印 token、完整环境变量或代理凭据。
2026-11-02 的初步发现见 ENVIRONMENT，后续会话必须核实是否变化。

## 2. 下载 Git（缺失时）

官方：https://git-scm.com/downloads/win
保留常规默认安装选项。安装后新开终端执行 git --version。
不需要为了本项目安装付费 Git 客户端。GitHub CLI 可选，已有则复用。

## 3. 下载 Android Studio Stable

官方：https://developer.android.com/studio
安装说明：https://developer.android.com/studio/install
选 Windows 稳定版安装器，不选 Canary/Beta 来尝鲜。
安装器需写系统目录时按当前环境权限执行；不能执行时把确切安装步骤交给用户，不声称已安装。

首次启动：
1. 完成 Setup Wizard。
2. SDK Manager 中安装一个与工程相符的稳定 Android SDK Platform。
3. 安装 Android SDK Platform-Tools（含 adb）与需要的 Build-Tools。
4. Command-line Tools（latest）可安装，便于后续诊断。
5. 真机优先：模拟器可选；需要模拟器才安装 Emulator 与一个匹配系统镜像。
6. 不安装所有历史 SDK、不安装 NDK/CMake，当前没有 native C++ 需求。

SDK 位置从 SDK Manager 读取，记录到本机环境，不把绝对路径写进共享构建脚本。
Windows 常见路径为 %LOCALAPPDATA%\Android\Sdk，但必须以实际设置为准。

## 4. 使用兼容的 JDK

优先使用 Android Studio 自带且与所选 AGP 兼容的 JBR。
Android Studio 设置中核对 Gradle JDK；终端构建也要使用同一兼容 JDK。
不要因为系统 java 指向 JDK 23 就默认沿用，也不要未经检查卸载其他项目的 JDK。

仅对当前 PowerShell 会话设置（把路径替换为实际存在的位置）：
~~~powershell
$fitnessJdk = 'C:\Program Files\Android\Android Studio\jbr'
if (-not (Test-Path -LiteralPath "$fitnessJdk\bin\java.exe")) { throw '请先核对 Android Studio JBR 的真实位置' }
$env:JAVA_HOME = $fitnessJdk
$env:Path = "$fitnessJdk\bin;" + $env:Path
java -version
~~~

环境变量路径属于本机信息，不提交 local.properties。
不单独下载安装全局 Gradle。使用项目自带 Gradle Wrapper。

## 5. 创建最小工程（T02）

先读任务卡，不要一开始创建全部页面。
Android Studio → New Project → Empty Activity（Compose 模板）：
- 名称：Fitness Journal
- 包名：com.molufengjun.fitnessjournal（暂定，创建时固定）
- 语言：Kotlin
- 构建脚本：Kotlin DSL
- minSdk：暂定 26；T03 检查真机 Android 版本后调整，已有数据前固定。
- compileSdk / targetSdk：使用当前稳定工具链支持的稳定 API，不使用预览版；记录实际数值。
- 项目位置：已克隆仓库根目录。若 IDE 不允许非空目录，先生成到临时空目录，再仅拷贝工程文件，不能覆盖 .git、AGENTS、README、docs。

完成首次 Gradle Sync。SDK 下载异常时先排查网络和源，不随意换未知镜像。
记录 Studio、AGP、Wrapper、Gradle JDK、Kotlin、Compose compiler、Compose BOM 的实际版本。
AGP 9+ 的内置 Kotlin 与旧插件配置有差异，禁止把不同年代教程拼成一份 build.gradle。

仓库根目录执行：
~~~powershell
.\gradlew.bat --version
.\gradlew.bat :app:assembleDebug
.\gradlew.bat :app:testDebugUnitTest
.\gradlew.bat :app:lintDebug
~~~

没有测试用例时必须写“任务可运行，但尚无业务测试”，不能当作业务验收。
跟踪 gradlew、gradlew.bat、gradle/wrapper 下文件（含 JAR）和 libs.versions.toml。

## 6. 连接小米手机（T03）

1. 用支持数据传输的 USB 线连接。
2. 在手机系统设置中找到开发者选项，开启 USB 调试；路径随 HyperOS 版本变化，以手机实际菜单为准。
3. 手机上确认本机 RSA 授权。
4. 若当前系统另有 USB 安装开关，只在需要时按实际提示处理；不需要解锁 Bootloader 或 root。
5. 在 Android Studio 选择该设备并 Run。

命令行诊断，先确认真实 SDK 路径：
~~~powershell
$fitnessSdk = Join-Path $env:LOCALAPPDATA 'Android\Sdk'
& "$fitnessSdk\platform-tools\adb.exe" devices -l
~~~

- device：可用。
- unauthorized：手机确认 USB 调试授权。
- offline：重新连接、核对 adb 服务和设备，不马上卸载 App。
- 完全没有设备：先换数据线/USB 模式，再查 Windows 驱动。Google USB Driver 不是所有小米设备通用驱动。
- 多台设备时后续命令加 -s DEVICE_ID，不能误装到其他设备。
- 公开日志去掉序列号。

手机成功展示空白模板页，返回再打开无崩溃后记录 T03 证据。
模拟器可以验证基础 UI，但不能代替小米省电/重启提醒测试。

## 7. 常见故障处理顺序

1. 找到第一条真实构建错误，不只看最后的 BUILD FAILED。
2. 确认项目路径、JDK 和 SDK。
3. 对照锁定的 AGP/Gradle/Kotlin/KSP/Room 兼容关系。
4. 网络错误与编译错误分开处理。
5. 修复后只重跑受影响命令。
6. 不默认 clean、不删除所有缓存、不卸载有真实数据的 App。

当前不需要下载：Node、Docker、PostgreSQL、Rails、Flutter、Figma 付费版或云服务客户端。
