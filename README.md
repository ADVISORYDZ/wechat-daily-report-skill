# WeChat Daily Report Generator (微信群聊日报生成工具)

这是一个用于分析微信群聊天记录，结合 AI 生成内容，并最终输出为精美手机端长图（PNG）的工具。

## ✨ 功能特点

- **数据统计**: 自动分析群聊记录，生成话唠榜、熬夜冠军、词云统计等数据。
- **AI 智能摘要**: 利用 AI 识别讨论热点、提取有价值的资源/教程、捕捉有趣对话和问答。
- **可视化报告**: 基于 HTML/CSS 模板渲染，自动生成适配手机屏幕（iPhone 14 Pro Max 分辨率）的日报图片。
- **风格化**: 支持幽默、玩梗的报告风格，提升阅读乐趣。

## 🛠️ 依赖环境

- Python 3.8+
- Node.js (可选，仅用于开发调试模板)

### Python 库安装

```bash
pip install jieba jinja2 playwright
playwright install chromium
```

## 🚀 使用流程

整个生成过程分为三个步骤：分析 -> AI 生成 -> 图片渲染。

### 1. 分析聊天记录

使用 `analyze_chat.py` 对原始聊天记录 JSON 进行初步清洗和统计。推荐使用 [WeFlow](https://github.com/hicccc77/WeFlow) 项目导出的 **ChatLab** 格式数据。

```bash
python scripts/analyze_chat.py <your_chat_log.json> --output-stats stats.json --output-text simplified_chat.txt
```

**输入**:
- `<your_chat_log.json>`: 符合格式要求的聊天记录文件 (见下文数据格式)。

**输出**:
- `stats.json`: 统计数据文件。
- `simplified_chat.txt`: 清洗后的纯文本聊天记录，用于投喂给 AI。

### 2. AI 生成内容

这一步需要将上一步生成的 `simplified_chat.txt` 内容提供给 AI (如 ChatGPT, Claude, Gemini)，并要求其按照 `references/ai_prompt.md` 中的提示词和格式生成 JSON 数据。

**操作指南**:
1. 打开 `references/ai_prompt.md` 复制提示词。
2. 将提示词和 `simplified_chat.txt` 的内容发送给 AI。
3. 将 AI 返回的 JSON 内容保存为 `ai_content.json`。

**AI 生成内容包括**:
- 话题摘要 (Topics)
- 资源分享 (Resources)
- 有趣对话 (Dialogues)
- 问答精选 (Q&A)
- 成员画像标签 (Talker Profiles)

### 3. 生成日报图片

使用 `generate_report.py` 将统计数据和 AI 内容合并渲染为图片。

```bash
python scripts/generate_report.py --stats stats.json --ai-content ai_content.json --output report.png
```

**输出**:
- `report.png`: 最终生成的日报图片。

## 📂 数据格式

### 输入聊天记录 JSON 结构

```json
{
  "meta": {
    "name": "群名称",
    "platform": "wechat",
    "type": "group"
  },
  "members": [
    {"platformId": "wxid_xxxx", "accountName": "用户A"}
  ],
  "messages": [
    {
      "sender": "wxid_xxxx",
      "accountName": "用户A",
      "timestamp": 1700000000,
      "type": 0,
      "content": "消息内容"
    }
  ]
}
```
*注：目前仅支持分析 `type: 0` (文本消息)。*

## 📁 项目结构

- `scripts/`: 核心 Python 脚本
    - `analyze_chat.py`: 数据清洗与统计
    - `generate_report.py`: 模板渲染与图片生成
- `assets/`: 资源文件
    - `report_template.html`: Jinja2 报告模板
- `references/`: 参考文档
    - `ai_prompt.md`: AI 提示词模板
- `SKILL.md`: 技能详细说明

## 📝 License

MIT
