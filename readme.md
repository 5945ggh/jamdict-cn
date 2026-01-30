# Jamdict-CN

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

Jamdict 日语词典的中文汉化版本, 使用 LLM 将 Jim Breen's JMdict 日英词典的英文释义逐条翻译成中文

## 免责声明
**本项目译文由 AI 模型生成，虽经大量逻辑优化，仍可能存在少量偏差。仅供参考，严谨学术场景请查阅原文。**

## 特性

- **完整汉化** - 19万+ 词条，37万+ 中文释义，100% 覆盖率
- **翻译语境** - 翻译过程中为 LLM 提供日语语境, 避免了英文一词多义可能产生的翻译问题
- **开源免费** - 遵循 JMdict 原始数据许可证


## 数据统计

| 指标 | 数值 |
|------|------|
| 词条总数 | 191,541 |
| 中文释义（完整版） | 373,939 |
| 中文释义（去重版） | 366,064 |
| 翻译覆盖率 | 100% |


## 快速开始

### 1. 安装 Jamdict 库

参考:
[https://github.com/neocl/jamdict](https://github.com/neocl/jamdict)
[https://pypi.org/project/jamdict/](https://pypi.org/project/jamdict/)

### 2. 下载数据库文件
从 **Releases** 下载压缩包, 解压后放到指定路径, 后续初始化 Jamdict 实例的时候将.db文件路径传入即可

我们提供两个版本：

1.  **Standard (推荐)** 
(`jamdict_cn_deduplicated.db`): 
    经过智能去重处理，适合大多数词典查询和 App 开发场景
2.  **Full** (`jamdict_cn.db`): 
    包含所有原始翻译结果（中英一对一映射），适合数据分析或作为原始语料库

### 使用示例
```python
from jamdict import Jamdict

# 使用中文化后的数据库
DB_PATH = "path/of/jamdict_cn.db" # 一对一地为每一个英文释义添加了中文释义
DEDUPLICATED_DB_PATH = "path/of/jamdict_cn_deduplicated.db" # 在每个义项内部做了中文释义的去重, 适合大多数使用场景

jam = Jamdict(db_file=DEDUPLICATED_DB_PATH)

# use wildcard matching to find anything starts with 食べ and ends with る
result = jam.lookup('食べ%る')

# print all word entries
for entry in result.entries:
    print(entry)

# output:
# [id#...] たべる (食べる) : 1. to eat/吃 (lang:chn) ((Ichidan verb|transitive verb)) 2. to live on (e.g. a salary)/to live off/to subsist on/靠...生活 (lang:chn)/依赖...生存 (lang:chn)/依靠...过活 (lang:chn) ((Ichidan verb|transitive verb))
# [id#...] たべすぎる (食べ過ぎる) : to overeat/吃得过多 (lang:chn) ((Ichidan verb|transitive verb))
# [id#...] たべつける (食べ付ける) : to be used to eating/习惯于吃 (lang:chn) ((Ichidan verb|transitive verb))
# [id#...] たべはじめる (食べ始める) : to start eating/开始吃 (lang:chn) ((Ichidan verb))
# [id#...] たべかける (食べ掛ける) : to start eating/开始吃 (lang:chn) ((Ichidan verb))
# [id#...] たべなれる (食べ慣れる) : to be used to eating/to become used to eating/to be accustomed to eating/to acquire a taste for/习惯吃 (lang:chn)/逐渐习 惯吃 (lang:chn)/对……习以为常 (lang:chn)/开始喜欢上（某种食物） (lang:chn) ((Ichidan verb))
# [id#...] たべられる (食べられる) : 1. to be able to eat/可食用 (lang:chn) ((Ichidan verb|intransitive verb)) 2. to be edible/to be good to eat/可食的 (lang:chn)/可食用 (lang:chn) ((pre-noun adjectival (rentaishi)))
# [id#...] たべくらべる (食べ比べる) : to taste and compare several dishes (or foods) of the same type/品尝并比较同类型的不同菜肴（或食物） (lang:chn) ((Ichidan verb|transitive verb))
# [id#...] たべあわせる (食べ合わせる) : to eat together (various foods)/一起食用（多种食物） (lang:chn) ((Ichidan verb))
# [id#...] たべあきる (食べ飽きる) : to get tired of eating/to have enough of (a food)/吃腻了 (lang:chn)/对某种食物感到厌倦 (lang:chn) ((Ichidan verb|transitive verb))


# 打印全部中文释义
entry = result.entries[0]
print(str(entry.kanji_forms) + ":")
num = 1
for sense in entry.senses:
    for gls in sense.gloss:
        if gls.lang == "chn": 
            print(f"{num}: {gls.text}")
            num += 1
            
#output: 
# [食べる, 喰べる]:
# 1: 吃
# 2: 靠...生活
# 3: 依赖...生存
# 4: 依靠...过活
```

## 版本说明
jamdict_cn.db 对每一条英文释义(gloss)都添加了对应的中文释义

jamdict_cn_deduplicated.db 对翻译结果进行了去重处理：

- **类型1（已删除）**: 同一 Sense 下的完全重复释义
  - 例如：接触恐怖症的 Sense 中 "触觉恐惧症" 重复 8 次

- **类型2（保留）**: 跨 Sense 的重复释义
  - 例如：から（kara）的不同 Sense 中都有 "因为"
  - 保留是因为每个 Sense 代表不同的含义/用法

## 致谢

- Jim Breen's JMdict - 词典数据来源
- [neocl/jamdict](https://github.com/neocl/jamdict) - 原始 Python 库

## 贡献

欢迎贡献！你可以：

1. 报告翻译错误
2. 提交翻译改进
3. 完善文档
