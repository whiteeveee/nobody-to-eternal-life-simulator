# Event Schema (MVP)

目标：把所有内容（随机/分支/突破/结局）统一成可机读结构，驱动引擎运行。

## 1. Core Types

### 1.1 EventType
- random：随机事件（无选择）
- choice：分支事件（A/B）
- breakthrough：突破节点（过会/问询/终审等，输出 PASS/FIX/FAIL/CLEAR 等）
- ending：结局（雷劫与终局输出）

### 1.2 Stage
- mortal：凡人
- xiuxian：修仙

### 1.3 Realm（修仙境界）
- qi / zhuji / jindan / yuanying / huashen
（凡人阶段 realm 可省略或用 "mortal"）

## 2. Gate（触发条件）
Gate 用于过滤候选事件：
- ageMin / ageMax / ageEq：年龄门槛
- stage / realm：阶段与境界门槛
- requires / forbids：字符串条件（flag、tag、item）
- pGte / avgGte / dLte：突破/雷劫等阈值（可选）

条件表达式建议（字符串）：
- flag:GF_RING=true
- item:雷劫应急包
- tag:灰产>=2
- stat:B>=30
（MVP 可先只实现 flag/item/tag）

## 3. Effects（统一效果）
Effect 是引擎唯一会执行的“指令集合”。

### 3.1 op 类型
- addStat: 属性增减（key, delta）
- clampStat: 限制范围（key, min, max）
- addItem / removeItem
- addTag / removeTag / incTag
- setFlag (key, value)
- setRealm (value)
- setAgeCap (value)

## 4. Event JSON 结构

### 4.1 random
字段：
- id, type, stage, realm?, gate?, text
- baseWeight
- effects[]
- tags?, note?

### 4.2 choice
字段：
- id, type, stage, realm?, gate?, text
- choices[]: { key:"A"/"B", text, effects[] }
- tags?, note?

### 4.3 breakthrough
字段：
- id, type, stage, realm:"transition" 或具体节点名
- gate
- text
- outcomes[]: { key:"PASS"/"FIX"/"FAIL"/"CLEAR", effects[] }

### 4.4 ending
字段：
- id, type:"ending"
- text
- metaReward: pointsMin/pointsMax, unlock[]
