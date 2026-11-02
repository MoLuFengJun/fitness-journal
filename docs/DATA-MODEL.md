# 数据模型 v0.1

此处为拟定实现合同；T07 建库前核对，一旦有真实数据，任何结构变更都走迁移。
所有示例必须虚构，不从其他会话读取真实背景来初始化 App。

## 通用约定

- 所有用户实体使用 UUID 字符串主键，离线生成。
- createdAt / updatedAt：UTC Instant；业务归属日另存 LocalDate（YYYY-MM-DD）。
- 同时保存记录当时的 zoneId；跨时区后不重新把过去午餐挪到另一天。
- 重量默认 kg；身高 cm；能量 kcal；食物净重 g；营养 g。
- DB 使用整数定点：身体/负重用克、身高用毫米、营养用毫克、能量用毫千卡；显示按需取小数。
- 纯计算边界统一转换；不在各页面随意混用克和公斤。
- Unknown 用 null，不用0。参数、份量和有限数检查在领域层统一执行。
- 归档食物/动作不删除已有记录；不需要网络 tombstone 或事件溯源系统。
- 用户可删除历史，先提供撤销/确认；恢复文件中的删除按快照替换语义处理。

## 实体与字段

| 实体 | 必需/主要字段 | 约束和用途 |
| --- | --- | --- |
| Profile | id=local、heightMm、ageYears、ageAsOfDate、equationSex、zoneId | 参数可缺省；年龄以用户确认值和日期保存，不推断生日 |
| AppPreferences | id=local、displayUnits、themeMode | 初版kg/cm/中文/明亮；避免提前实现多套单位 |
| BodyMeasurement | id、measuredAt、localDate、zoneId、weightG、bodyFatBasisPoints?、waistMm?、source、note? | 同一天可多条；日趋势取当天最后一条有效记录 |
| GoalPeriod | id、effectiveFrom、goalType、targetKcalMilli、proteinMg、fatMg、carbMg、maintenanceKcalMilli?、calculationVersion、inputSnapshotJson | 生效日期唯一；不使用当前目标改写过去 |
| FoodDefinition | id、name、basisType、basisQuantity、basisUnit、energyKcalMilli、proteinMg、fatMg、carbMg、sourceType、sourceNote?、archivedAt? | 每100g或每份两种基准；“一个”需对应明确份量 |
| MealTemplate | id、name、archivedAt? | 套餐定义 |
| MealTemplateItem | id、templateId、foodId、quantity | 模板更新只影响以后添加 |
| FoodEntry | id、localDate、occurredAt?、zoneId、mealSlot、foodId?、nameSnapshot、quantity、basisSnapshot、nutrientSnapshot、sourceSnapshot、note? | 加入日记时复制名称/单位/营养快照 |
| DayLogState | localDate、nutritionComplete、trainingReviewState | 未记录不等于没吃或休息；trainingReviewState=UNKNOWN/REST/RECORDED |
| ExerciseDefinition | id、name、equipmentLabel、loadKind、loadConvention、muscleTags、archivedAt? | 不同器械/计重口径不同身份 |
| WorkoutTemplate | id、name、archivedAt? | 简单可复用动作列表 |
| WorkoutTemplateItem | id、templateId、exerciseId、sortIndex、suggestedSetCount | 仅参考，不能预标完成 |
| WorkoutSession | id、startedAt、endedAt?、localDate、zoneId、status、note? | DRAFT/COMPLETED；保存草稿不依赖结束按钮 |
| WorkoutExercise | id、sessionId、exerciseId?、sortIndex、nameSnapshot、equipmentSnapshot、loadKindSnapshot、loadConventionSnapshot | 冻结历史比较身份与口径 |
| WorkoutSet | id、workoutExerciseId、ordinal、loadG?、reps?、rir?、setKind、completedAt? | 只有已完成工作组参与主要比较；热身单列 |
| ActivityEntry | id、localDate、zoneId、activityType、durationSeconds、metValue?、metSource?、estimatedGrossKcalMilli?、calculationVersion、workoutSessionId? | 同一训练能耗只记一次；可空而非伪造 |
| ReminderSchedule | id、label、localTime、zonePolicy、daysOfWeek、enabled、updatedAt | label用户自己输入；zonePolicy初版FOLLOW_DEVICE |
| ReminderOccurrence | id、scheduleId、scheduledLocalDateTime、zoneId、status、actedAt?、snoozedUntil? | 同一计划+发生时刻唯一；PENDING/DONE/SKIPPED |
| BackupEvent | id、createdAt、direction、result、schemaVersion、recordCounts | 仅设备运维状态，不存文件正文；不等于云同步状态 |

## 关系与一致性

- 套餐添加到日记：一个事务插入所有 FoodEntry，避免只插入一半。
- 训练结束：一个事务更新 session 状态与相关计数所需字段。
- 修改动作名称可保留 ID；改变器械/助力种类/负重单位时创建新定义或明确新比较版本。
- 提醒完成动作以 occurrence 唯一键幂等写入；重复点击不多记一次。
- 目标的输入快照包括当时体重、身高、年龄、活动系数、缺口参数；不得因体重后来改变重算旧目标。
- 食物模板依赖 FoodDefinition；归档被引用食物时模板标记需处理，不静默替换。
- 删除正在被引用的字典条目使用归档，不级联清空日记/训练。

## 当日与历史口径

- 统计区间用日期闭区间，最后一天包括全天。
- 体重趋势每日一个代表点，保留全部原始测量可查看。
- 最近一次训练：同一比较身份下，最近的已完成 session，不包括今天未结束草稿。
- 历史最好：同负重看次数；同次数看负重；助力动作负重方向反转。
- 不把不同器械公斤数、单只与总重、助力与负重混在同一曲线。
- 统计派生值可重算，不用创建一张每日汇总表来同步维护。

## 验证与版本

保存 schema JSON 到 app/schemas。
迁移测试使用虚构旧数据，验证记录数量、ID、关键字段与关系不变。
备份 schemaVersion 与 Room DB version 是两套独立版本，不能混用。
