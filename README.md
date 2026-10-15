# Academic Outreach Skills

一组用于博士或研究型学位申请前期调研的 Codex skills：先从大学院系官网整理导师信息，再结合申请者简历评估匹配度并生成个性化 cold email 草稿。

> 本仓库只生成研究资料和邮件草稿，不会自动发送邮件。导师信息与研究方向应以学校官网和可核验来源为准。

## Skills

| Skill | 功能 | 主要输出 |
|---|---|---|
| [`faculty-scraper`](skills/faculty-scraper/SKILL.md) | 从院系官网收集、清洗、去重并核验导师信息 | 导师名单 Excel/CSV |
| [`cold-email-writer`](skills/cold-email-writer/SKILL.md) | 读取 CV 与导师名单，评估匹配度并撰写个性化英文邮件草稿 | 匹配等级、理由和邮件草稿 |

两个 skill 可以独立使用，也可以按顺序组成完整工作流：

1. 使用 `$faculty-scraper` 建立或更新导师名单。
2. 人工抽查姓名、邮箱、职位、研究方向和来源链接。
3. 使用 `$cold-email-writer` 读取 CV，完成匹配评估和邮件草稿。
4. 逐封人工审阅事实、措辞和收件人，再自行发送。

## 这次整理包含什么

- 移除了固定姓名、固定文件路径、固定导师数量和特定学校结构等个人化假设。
- 将详细字段规范和写作规则拆分到 `references/`，减少 skill 默认加载的上下文。
- Excel 操作改为按表头识别字段，并要求保留公式、样式、合并单元格和已有人工记录。
- 增加来源记录、去重、访问边界、失败标记和抽样核验要求。
- 邮件生成增加“不得捏造论文、项目或研究交集”的事实约束。
- 增加 Codex UI 元数据、仓库级隐私忽略规则和贡献说明。

## 安装

克隆仓库后，将两个 skill 文件夹复制或链接到 Codex 的 skills 目录：

```bash
git clone https://github.com/longxiangyi8-hub/academic-outreach-skills.git
mkdir -p ~/.codex/skills
cp -R academic-outreach-skills/skills/faculty-scraper ~/.codex/skills/
cp -R academic-outreach-skills/skills/cold-email-writer ~/.codex/skills/
```

如需运行示例中常用的 Python 工作流，可安装这些可选依赖：

```bash
python3 -m pip install -r academic-outreach-skills/requirements.txt
```

安装后新建一个 Codex 任务，直接点名 skill 即可。

## 使用示例

### 1. 整理导师名单

```text
使用 $faculty-scraper，从以下院系官网整理所有 Assistant Professor、
Associate Professor 和 Professor。写入“选导师.xlsx”的“导师名单”工作表，
保留已有的进度和备注，并记录每条信息的来源链接：<院系官网 URL>
```

### 2. 评估匹配并生成邮件

```text
使用 $cold-email-writer，读取“申请者CV.pdf”和“选导师.xlsx”。
申请目标为 2027 Fall PhD；如暂时没有正式名额，可以表达对 RA 的兴趣。
请写入匹配等级、匹配理由和英文邮件草稿，不要发送邮件。
署名使用 <Applicant Name>。
```

## 输入与输出

推荐的导师表字段如下。现有表格可以使用不同列顺序，skill 会优先根据表头映射，而不是依赖固定列号。

| 字段 | 是否推荐 | 说明 |
|---|---:|---|
| 地区 | 是 | 国家、地区或城市 |
| 学校 | 是 | 大学全称 |
| 学院 | 是 | 学院、系或研究中心 |
| 教师 | 是 | 导师姓名 |
| 邮箱 | 是 | 官方邮箱；没有公开邮箱时留空 |
| 职位 | 是 | 官网展示的职称 |
| 方向 | 是 | 可核验的研究兴趣或关键词 |
| 进度 | 可选 | 由申请者维护，如“待联系”“已投” |
| 备注 | 可选 | 人工记录 |
| 来源 | 是 | 院系列表页或个人主页 URL |
| 抓取时间 | 是 | 信息核验日期 |
| 匹配度 | 第二阶段 | High / Medium / Low |
| 匹配理由 | 第二阶段 | 简短、可追溯的判断依据 |
| 套瓷信 | 第二阶段 | 英文邮件草稿 |

更完整的字段和写作规范见各 skill 的 `references/` 目录。

## 隐私与使用边界

- 不要把真实 CV、成绩单、私人邮箱、已填写的申请表或生成后的导师名单提交到仓库。
- 只采集公开且与申请调研直接相关的职业信息；尊重网站条款、`robots.txt`、访问频率限制和登录边界。
- 不绕过验证码、付费墙、访问控制或反自动化保护。遇到限制时记录缺失项并改用公开来源或人工核验。
- 自动生成的匹配度只是筛选辅助，不应替代申请者对导师近期论文、招生状态和研究契合度的判断。
- 发送前逐封检查称呼、事实、研究内容、附件和收件地址。

## 仓库结构

```text
academic-outreach-skills/
├── README.md
├── CONTRIBUTING.md
├── LICENSE
├── requirements.txt
└── skills/
    ├── faculty-scraper/
    │   ├── SKILL.md
    │   ├── agents/openai.yaml
    │   └── references/output-schema.md
    └── cold-email-writer/
        ├── SKILL.md
        ├── agents/openai.yaml
        └── references/email-guidelines.md
```

## 质量检查

每次修改 skill 后，可使用 Codex 自带的 skill validator 检查目录名、YAML frontmatter 和未清理的脚手架内容：

```bash
python3 /path/to/skill-creator/scripts/quick_validate.py skills/faculty-scraper
python3 /path/to/skill-creator/scripts/quick_validate.py skills/cold-email-writer
```

validator 只检查结构。提交前还应确认：说明中的相对链接有效、示例不包含个人数据、两个 skill 的字段命名一致。

## License

[MIT](LICENSE)
