# 谣言粉碎机 - 基础用法示例

## 快速入门

### 1. 文本真实性核查

**输入示例：**
```
帮我查一下这个消息是真的还是假的：
"紧急通知！今天晚上请不要开窗户，因为政府正在大面积喷洒消毒药。
请大家互相转告！这不是谣言！"
```

**调用方式：**
```python
# 使用核心引擎分析
from rumor_crusher_core import RumorCrusherCore

core = RumorCrusherCore()
result = core.analyze_text_content(
    "紧急通知！今天晚上请不要开窗户，因为政府正在大面积喷洒消毒药。",
    mode='standard'
)
print(f"评分: {result['final_score']}/100")
print(f"判定: {result['verdict']}")
```

---

### 2. 图片真实性验证

**输入示例：**
```
帮我查一下这张图是真的还是P的
```

**调用方式：**
```python
from image_forensics import ImageForensics

fi = ImageForensics()
result = fi.analyze_image('/sdcard/Download/suspected_image.jpg')
print(result['verdict'])
```

---

### 3. 来源追溯

**输入示例：**
```
帮我追查一下这条信息的源头
```

**调用方式：**
```python
from source_tracer import SourceTracer

tracer = SourceTracer()
report = tracer.trace_source("待追溯的文本内容")
print(f"找到 {report['source_count']} 个相关来源")
print(f"最早来源: {report['earliest_source']}")
```

---

### 4. 典型场景示例

#### 场景一：家族群养生谣言
```
用户: 帮我看看这条消息真的假的
"注意了！菠菜和豆腐一起吃会产生结石！转给家人朋友！"

核查结果：
🔴 确认谣言 (评分: 18/100)
- 检测到「绝对化断言」「伪科学医疗建议」模式
- 菠菜和豆腐同食不会产生结石
- 辟谣来源：中国互联网联合辟谣平台
```

#### 场景二：朋友圈爆款文章
```
用户: 这篇说"99%的人都不知道的致癌真相"，是真的吗？

核查结果：
🟠 高度存疑 (评分: 38/100)
- 检测到「标题党」「伪数据恐吓」模式
- 大量使用"专家说"但无具名专家
- 数据来源无法核实
```

#### 场景三：投资群项目推荐
```
用户: 这个"稳赚不赔月收益30%"的项目靠谱吗？

核查结果：
🔴 确认谣言 (评分: 12/100)
- 检测到「金融骗局」「虚假承诺」
- 承诺高回报率是典型庞氏骗局特征
- 建议立即停止参与并向有关部门举报
```

---

## 高级功能

### 批量核查
```python
# 批量核查多条信息
texts = [
    "第一条可疑信息...",
    "第二条可疑信息...",
    "第三条可疑信息..."
]

for text in texts:
    result = core.analyze_text_content(text, mode='quick')
    print(f"[{result['verdict']}] {text[:30]}...")
```

### 自定义规则
```python
# 可扩展自定义谣言模式
custom_pattern = {
    'name': '自定义模式',
    'keywords': ['自定义关键词1', '自定义关键词2'],
    'severity': 'high'
}
```

---

## 输出格式对比

| 模式 | 耗时 | 详细程度 | 适用场景 |
|-----|:----:|:--------:|---------|
| `quick` (快速) | < 5秒 | 简要判定 | 日常消息快速核实 |
| `standard` (标准) | < 15秒 | 详细分析 | 需要了解具体问题 |
| `deep` (深度) | < 30秒 | 完整溯源 | 重要信息/传播分析 |