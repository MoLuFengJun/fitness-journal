# 双环境索引

更新：2026-11-03。项目共用一份 [STATUS](STATUS.md)，每台电脑分别记录工具、网络和验证结果：
- [COMPANY：公司环境](environments/COMPANY.md)
- [HOME：家用环境](environments/HOME.md)
- [跨电脑与同步操作手册](../MULTI-ENV-WORKFLOW.md)

2026-11-02用户明确确认当时是COMPANY。**这是历史事实，不能据此默认以后每轮仍在公司。**

## 每次开工先识别环境

按优先级执行：
1. 用户本轮明确说公司/家里时，直接采用，不重复询问。
2. 否则检查当前工作副本中的本机文件 .local/fitness-journal.environment.json；仅在repository匹配、machineBinding与当前主机匹配且没有迁移迹象时采用。
3. 同一轮已确认且执行主机没变可以沿用。新的一天、明确切换设备或执行器变更后，不凭旧聊天认定位置。
4. 没有可靠依据则简短询问“这次用公司电脑还是家里电脑？”；只凭目录、JDK版本、网络失败或最新远程会话记录不能确定。
5. 识别期间可处理与本机无关的远程文档；安装、构建、设置本机标记等操作等待环境确认。

开始环境相关操作前说明envId及识别依据。标记只证明设备归属，不证明工具/网络仍可用。

## 本机标记

在环境确认后，由执行模型将标记写入当前项目工作副本的 .local/fitness-journal.environment.json。
若当前只有会话工作区而非checkout，可放工作区.local中，但workspaceRole必须如实填写。
本标记不会自动随本次远程文档生成；本次没有在未确认的机器上新建标记。

~~~json
{
  "schemaVersion": 1,
  "repository": "MoLuFengJun/fitness-journal",
  "environmentId": "COMPANY",
  "confirmedOn": "2026-11-02",
  "confirmedBy": "user",
  "machineBinding": "仅在本机填写实际主机名，禁止上传",
  "workspaceRole": "checkout"
}
~~~

- .local/已被.gitignore排除，不跨电脑复制标记。
- machineBinding使用本机已有OS主机名即可，不采集IP/MAC/公司域/证书。
- 若标记与机器不匹配，询问并重新绑定；不悄悄假定HOME或COMPANY。
- 第三台电脑先确认，再新增独立档案，不覆盖这两台历史。
- 私人路径、代理信息、签名位置只留本机；公开档案只记版本和脱敏结果。

## 两套环境检查

| 检查 | COMPANY | HOME |
| --- | --- | --- |
| E00 设备归属 | 2026-11-02用户确认；新会话按识别规则 | 未确认 |
| E01 本机checkout与远程基线 | 当前未验证可用checkout；此前工作区不是Git仓库 | UNKNOWN |
| E02 工具/兼容版本 | 部分盘点，Android未就绪 | UNKNOWN，可能需要从头安装 |
| E03 指定提交构建 | NOT_RUN | NOT_RUN |
| E04 手机连接与运行 | NOT_RUN | NOT_RUN |
| GitHub连接器 | 2026-11-02读写成功；每轮检查能力是否提供 | UNKNOWN |
| 本地gh请求 | 2026-11-02代理连接被拒 | UNKNOWN |
| 本地clone/fetch/push | 未实测；用户报告push常失败 | UNKNOWN |

UNKNOWN不等于未安装；NOT_RUN不等于失败；用户报告不等于工具实测。
环境改变时更新对应档案和本表，保留日期与证据，不能用HOME的PASS填COMPANY。

## 全局任务与本机准备分开

T00–T29只维护一份，不按电脑复制。新电脑补E01–E04，不把已完成项目任务清零。
T02创建Android工程只做一次；第二台导入现有工程，按Wrapper/版本目录安装兼容JDK/SDK。
项目内构建文件是共享版本真相；安装路径、缓存、local.properties属于各机器。
不要为新电脑全量升级依赖，也不以卸载有数据App解决两台机器签名不一致。
