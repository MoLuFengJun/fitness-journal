# 两台电脑协作与可靠同步

更新：2026-11-03。COMPANY/HOME共享项目进度，各自记录环境。
这里处理代码仓库同步，不是App个人数据云备份；后者T29仍待定。

## 开工顺序

1. 读AGENTS、START-HERE、STATUS、ENVIRONMENT索引。
2. 用户本轮已说明环境则采用；否则检查匹配本机的.local标记，仍不确定就询问。
3. 读对应COMPANY/HOME档案，检查当前会话是否提供GitHub连接器。
4. 若有checkout，先检查本地：
~~~powershell
git status --short --branch
git remote -v
git branch --show-current
git log -5 --oneline
~~~
5. 读取目标远程分支最新SHA，确定当前基线。不能凭旧STATUS断定另一台没更新。
6. 检查任务所需本机E01–E04；文档编辑不强求SDK，代码构建则必须具备。
7. 开始一个明确任务，记录envId、基线SHA和验证范围。

不建立永久company/home代码分支。共享基线main；未完成任务使用task/Txx-name或明确的WIP分支。

## 通道选择与重试规则

| 条件 | 首选 |
| --- | --- |
| COMPANY，网络条件未变化且连接器可用 | 连接器读取与提交文档/小规模文本 |
| HOME，普通Git已验证可用 | 普通Git clone/fetch/pull/push |
| 环境或能力未知 | 先识别，再做必要的最小只读核验 |
| 本地Git受阻但连接器可用 | 在其能力范围内工作，不能假称有本地checkout |
| 两种通道都不可用 | 保留工作并明确待同步，不宣称远程完成 |

- 已知失败且条件未变化，同一路径不再重复探测。
- 新失败做一次有依据的诊断：代理连接被拒是网络问题；授权失败才处理授权；non-fast-forward/lease失败是历史冲突。
- 不循环更换gh、curl、Git HTTPS、SSH碰运气，不擅自修改公司代理或关闭TLS验证。
- 只有用户说明网络变化、配置确已改变或明确要求重测，才重新尝试旧失败路径并记录。
- 连接器成功不证明Google Maven、Gradle或Android SDK下载可用，这些通道独立记录。
- 当前工具列表没有连接器时如实说明，历史成功不是本轮可用证明。

## 成功经验：2026-11-02公司环境

本地gh api user返回proxyconnect/套接字访问被拒。公开文档不保存内部地址和凭据。
用户另行报告git push经常失败；本轮未实际执行clone/fetch/push，不能把它们写成工具实测失败。

真正成功的通道是GitHub连接器：
1. 空仓库用Contents API创建README，初始化默认main。
2. 读取父提交和tree。
3. 基于原tree写入28个新增文件。
4. 创建父提交明确的新commit。
5. expected_sha指向原head、force=false更新main。
6. 核对远程head，并回读全部29个文件，逐个内容一致。

初始计划SHA：1a17ab866f8c870ef4969907f3e59f9cb83803f1。

**这不是本地git push成功。** API提交不会上传本地.git历史、未提交文件或构建缓存，也不证明本机已经clone。
当前main已经存在，后续不重复初始化，不清空仓库。

## 后续模型如何找到工具

查询当前工具目录中的github相关工具，再阅读实际参数：
- github_fetch：读取ref、Git commit/tree。
- github_fetch_file：读取路径与指定ref内容。
- github_create_tree：基于原tree构造改动。
- github_create_commit：创建带正确parent的新提交。
- github_update_ref：以expected_sha校验旧head，非强制更新。
工具前缀/参数可能变化，以本轮工具定义为准；不凭本文发明工具调用。

## 连接器多文件提交操作单

1. 读取目标分支head，记H；读取对应tree，记B。
2. 按H读取AGENTS及所有要编辑文件，保护另一台机器的新内容。
3. 准备本次差异，检查范围、链接与私人数据。
4. create_tree的base_tree_sha=B，仅传本次变化的条目。普通文本使用path、mode=100644、type=blob、content；已有执行位应保留，不误删无关文件。
5. create_commit的parent_sha=H、tree_sha=新tree。
6. update_ref指定目标branch、新SHA、expected_sha=H、force=false。
7. 如果head改变，停止更新，重读远程并在新基线上重做本次差异。不能force覆盖。
8. 回读ref核对SHA；回读改动文件验证内容；只有通过后才说同步成功。
9. 记录通道、远程分支/SHA及尚未同步内容。

相关多文件改动一次提交，避免逐文件留半套文档。
文本content方式不适用于APK、Wrapper JAR等二进制。需要支持的二进制blob接口或普通Git；工具不足标阻塞，不用文本冒充二进制。
本文档维护按用户授权直接更新main；后续功能代码遵守任务分支与验证门槛。

## 普通Git路径

仅在当前通道可用时执行，先保护已有工作：
~~~powershell
git fetch origin
git status --short --branch
git log --oneline --left-right HEAD...origin/main
~~~

在main、工作区干净、没有未推送工作且能快进时：
~~~powershell
git pull --ff-only origin main
~~~

任务分支完成授权的提交后：
~~~powershell
git push -u origin task/Txx-short-name
~~~

示例分支名需替换为真实当前分支；已有任务继续原分支，不机械新建。
推送后核对远程ref。拒绝快进时先处理分歧，不把它当网络失败，不force。

## API提交与本地Git历史对齐

API创建的commit通常和本地commit SHA不同；“文件内容已上传”不等于“本地commit已推送”。
- 本地无未提交/未推送工作且可快进：网络恢复后fetch、pull --ff-only。
- 本地有工作：先保存到独立本地分支/提交或可恢复副本，不reset --hard，不清空未跟踪文件。
- 本地与远程分叉：保留原分支，基于最新远程建对齐分支，只移植未同步的变更。已通过API上传的部分先比内容，避免重复应用。
- 冲突逐文件处理，不整体选ours/theirs。核对后再更新状态。
- 本地无法fetch时，不伪造.git对象来假装对齐；远程文档可继续，本地编码分支明确待对齐。

交接填写localCommit（如有）、remoteCommit、已同步路径、未同步路径与目标远程分支。
细节可保存在.local/sync-state.json，不入Git；公开记录只留必要任务与SHA。

## 换电脑前后

离开前：
1. 检查工作区已提交/未提交/未跟踪部分。
2. 更新STATUS、本机环境档案、会话记录。
3. 已完成任务按授权同步；未完成但需交接的改动可存明确WIP任务分支，记录未验证项，不标DONE。
4. 核对远程分支/SHA，再写下一电脑的第一步。
5. 没有同步成功时明确另一台还拿不到这些改动；不要把“已存在本机”当作远程可续。

到另一台后：
1. 识别环境，读取远程最新状态及交接。
2. 保护本地改动并对齐正确分支。
3. 缺工具仅补本机E01–E04，不能重置全局任务。
4. 已有Android工程直接导入，不再生成模板。
5. 按仓库锁定版本构建并记录本机结果，再接续当前任务。

## 版本、路径、签名边界

- Wrapper和依赖版本共享；SDK/JDK路径、缓存、local.properties各自保留。
- 新电脑就绪不需要全量升级项目依赖。
- 两台电脑默认debug密钥可能不同。覆盖安装前核对包名/签名，不卸载有记录App来解决冲突。
- 长期自用release更新使用同一私有签名密钥；由用户通过安全私人渠道提供给需要构建的电脑，绝不进Git。
- 无release密钥的电脑可做debug和测试，不能宣称可发布兼容更新。
- 个人健康数据、App备份、代理凭据与密钥不通过公开仓库跨电脑传递。
