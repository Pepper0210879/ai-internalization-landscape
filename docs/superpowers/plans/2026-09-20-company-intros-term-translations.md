# Company Intros and Term Translations Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 在现有厂商对照表中为全部 36 条记录增加简短机构简介，并为英文原始术语增加中文参考译名。

**Architecture:** 继续保持单文件静态网页结构，在现有 `data` 数组中加入 `vendorIntro` 和 `termZh` 字段，并在原有两个单元格中渲染第二行辅助文字。搜索沿用对象值拼接，因此新增字段自动进入检索范围；不引入依赖或新交互。

**Tech Stack:** HTML、CSS、原生 JavaScript、Node.js 静态校验、GitHub Pages。

## Global Constraints

- 企业简介位于名称下方，控制在 20–40 个中文字符左右，说明机构类型与核心业务。
- 英文原始术语保留为主文本，中文内容标注为“参考译名”。
- 纯中文术语或只包含通用缩写 `AI`、`ISO`、`IEC`、`X` 的中文术语不重复显示译名。
- 不增加表格列，不修改数据指标、定义、门槛、不包含项与来源链接。
- 桌面、移动与打印视图均保持自然换行和可读层级。

---

### Task 1: 扩充内容数据并渲染辅助文字

**Files:**
- Modify: `outputs/ai-internalization-vendor-landscape.html`

**Interfaces:**
- Consumes: 现有 `data: Array<{vendor, term, region, family, level, evidence, definition, threshold, excludes, url}>`。
- Produces: 每项新增 `vendorIntro: string` 与 `termZh: string`；表格渲染 `.vendor-intro` 和 `.term-zh`。

- [ ] **Step 1: 运行修改前校验，确认新增字段尚不存在**

Run:

```bash
node -e 'const fs=require("fs");const s=fs.readFileSync("outputs/ai-internalization-vendor-landscape.html","utf8");const d=JSON.parse(s.match(/const data=(\[.*?\]);\nconst metricData=/s)[1]);if(d.some(x=>"vendorIntro" in x||"termZh" in x))process.exit(1);console.log(`baseline:${d.length}`)'
```

Expected: `baseline:36`。

- [ ] **Step 2: 为 36 条记录加入以下内容**

同一厂商的重复条目必须复用相同简介：

```text
Microsoft | 全球企业软件与云服务公司，提供 Azure、Microsoft 365 等产品 | 前沿企业
OpenAI | 人工智能研究与产品公司，开发 ChatGPT 及企业 AI 平台 | AI 原生组织
Salesforce | 客户关系管理与企业云软件公司，推出 Agentforce 平台 | 智能体企业
Google Cloud | Google 旗下云计算与企业 AI 平台 | 智能体企业
SAP | 德国企业管理软件公司，核心产品覆盖 ERP 与业务流程 | 自治企业
ServiceNow | 企业工作流与数字运营平台公司 | AI 原生企业 / 智能体型业务
Workday | 云端财务与人力资本管理软件公司 | 智能体劳动力
UiPath | 企业自动化软件公司，专注 RPA 与智能体编排 | 智能体企业
Automation Anywhere | 企业自动化平台公司，提供 RPA 和智能体自动化 | 自治企业
AWS | Amazon 旗下云计算平台，提供基础设施与生成式 AI 服务 | AI 优先组织
NVIDIA | 芯片与加速计算公司，提供 AI 算力、软件和企业平台 | 企业 AI 工厂
Oracle | 数据库与企业软件公司，也提供云基础设施和业务应用 | 智能体企业
IBM | 企业科技与咨询公司，覆盖混合云、AI 与大型机 | AI 原生
Deloitte（两条） | 全球专业服务机构，提供审计、咨询、税务等服务 | AI 优先公司；AI 驱动型组织
EY | 全球专业服务机构，提供审计、咨询、税务与战略服务 | AI 原生企业
KPMG | 全球专业服务机构，提供审计、税务和咨询服务 | AI 优先企业
PwC | 全球专业服务机构，提供审计、咨询和税务服务 | 智能体企业
McKinsey | 全球管理咨询公司，服务战略、组织与运营转型 | 智能体组织 / 超级能动性
BCG（两条） | 全球管理咨询公司，专注战略、组织与数字化转型 | AI 优先组织；仿生企业
Accenture | 全球专业服务公司，提供咨询、技术与运营服务 | 全面企业重塑 / AI 优先重塑
Gartner | 信息技术研究与咨询机构，以市场研究和分析框架见长 | 自治业务
Anthropic | 人工智能安全与产品公司，开发 Claude 模型并研究可信智能体 | 可信智能体（边界）
Lenovo 联想 | 全球个人电脑与智能设备企业，布局基础设施与企业 AI | （不显示）
腾讯云 | 腾讯旗下云计算与产业互联网平台 | （不显示）
百度 | 中国互联网与人工智能公司，覆盖搜索、云服务与大模型 | （不显示）
CodeBanana / 出门问问 | 出门问问推出的 AI 原生开发与组织实践平台 | 超级组织
金蝶 | 企业管理云与财务软件公司，服务企业数字化与 AI 转型 | （不显示）
用友 | 企业软件与云服务公司，覆盖财务、人力和业务管理 | （不显示）
钉钉 | 阿里巴巴旗下企业协同办公与组织管理平台 | （不显示）
飞书 | 字节跳动旗下企业协作与办公平台 | （不显示）
华为云 | 华为旗下云计算与企业 AI 服务平台 | 企业智能体 / 智能体基础设施
Moka | 人力资源管理软件公司，提供招聘与组织管理产品 | （不显示）
ISO | 国际标准化组织，负责制定跨行业国际标准 | （不显示）
NIST | 美国国家标准与技术研究院，制定技术测量与风险框架 | 人工智能风险管理框架
```

对“不显示”的项目写入空字符串 `termZh: ""`。

- [ ] **Step 3: 添加紧凑的辅助文字样式**

在现有 `.vendor`、`.metric-note` 附近加入：

```css
.vendor-name{font-weight:700}.vendor-intro,.term-zh{display:block;margin-top:4px;color:var(--muted);font-size:12px;line-height:1.45;font-weight:400}.term-original{font-weight:600}
```

打印样式中加入：

```css
.vendor-intro,.term-zh{font-size:8px;line-height:1.3}
```

- [ ] **Step 4: 更新两列渲染，并让空译名不产生空行**

将行模板的前两个单元格改为：

```javascript
<td class="vendor"><span class="vendor-name">${esc(x.vendor)}</span><span class="vendor-intro">${esc(x.vendorIntro)}</span></td><td><span class="term-original">${esc(x.term)}</span>${x.termZh?`<span class="term-zh">参考译名：${esc(x.termZh)}</span>`:""}</td>
```

- [ ] **Step 5: 运行完整静态校验**

Run:

```bash
node - <<'NODE'
const fs=require('fs');
const s=fs.readFileSync('outputs/ai-internalization-vendor-landscape.html','utf8');
const d=JSON.parse(s.match(/const data=(\[.*?\]);\nconst metricData=/s)[1]);
const m=JSON.parse(s.match(/const metricData=(\[.*?\]);\nconst metricsByKey=/s)[1]);
const errors=[];
if(d.length!==36) errors.push(`data=${d.length}`);
if(m.length!==36) errors.push(`metrics=${m.length}`);
if(d.some(x=>!x.vendorIntro||x.vendorIntro.trim().length<10)) errors.push('missing/short vendorIntro');
const fullEnglish=d.filter(x=>/[A-Za-z]{3,}/.test(x.term.replace(/\b(?:AI|ISO|IEC)\b/g,'')));
if(fullEnglish.some(x=>!x.termZh)) errors.push('missing termZh');
if(!s.includes('class="vendor-intro"')||!s.includes('参考译名：')) errors.push('rendering missing');
if(errors.length){console.error(errors.join('\n'));process.exit(1)}
console.log(`PASS data=${d.length} metrics=${m.length} translated=${fullEnglish.length}`);
NODE
```

Expected: 以 `PASS data=36 metrics=36` 开头且退出码为 0。

- [ ] **Step 6: 在本地浏览器检查页面**

打开 `outputs/ai-internalization-vendor-landscape.html`，确认名称与简介、英文术语与参考译名层级清楚，数据指标下拉和搜索仍可用。

### Task 2: 发布并验证 GitHub Pages

**Files:**
- Update: GitHub 仓库根目录 `index.html`

**Interfaces:**
- Consumes: 通过 Task 1 校验的本地 HTML。
- Produces: `https://pepper0210879.github.io/ai-internalization-landscape/` 上的更新页面。

- [ ] **Step 1: 使用 GitHub contents API 更新根目录 `index.html`**

提交消息：

```text
Add company intros and Chinese term translations
```

- [ ] **Step 2: 等待 GitHub Pages 工作流成功**

Expected: 最新 `Deploy website to GitHub Pages` 工作流状态为 `completed`，结论为 `success`。

- [ ] **Step 3: 验证线上页面内容**

Run:

```bash
curl -L --fail --silent --show-error --max-time 20 \
  https://pepper0210879.github.io/ai-internalization-landscape/ \
  | rg '全球企业软件与云服务公司|参考译名：前沿企业'
```

Expected: 两段新增文字均被匹配，命令退出码为 0。
