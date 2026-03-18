---
name: Westernacher-PPT
description: "Generate PPT slide images using the NanoBanana API (Gemini image generation). Use this skill whenever the user asks to generate PPT images, create slide visuals, make presentation graphics, or mentions NanoBanana. Also trigger when the user provides detailed PPT layout descriptions and wants them turned into actual images. Covers single and batch (parallel Agent) generation of consulting-style PPT slides."
---

# Westernacher PPT Image Generator

Generate high-quality PPT slide images from detailed text descriptions using the NanoBanana API (OpenAI-compatible endpoint with Gemini image generation models).

## API Configuration

- **Endpoint**: Must be provided by the user at runtime (OpenAI-compatible chat completions endpoint)
- **Model**: `gemini-3.1-flash-image-preview-2K` (default, 2K resolution)
- **Alternative models**: `gemini-3.1-flash-image-preview` (standard), `gemini-3.1-flash-image-preview-4K` (4K)
- **API Key**: Must be provided by the user at runtime
- At the start of the conversation, if the user hasn't provided endpoint or key, ask: "Please provide your NanoBanana API Endpoint and API Key"

## How It Works

The API is OpenAI chat completions compatible. Send the PPT description as a user message, and the model returns a markdown image link with the generated image URL. Download the image to save it locally.

## Workflow

### Step 1: Collect Inputs

Gather from the user:
1. **API Endpoint** (required, ask if not provided)
2. **API Key** (required, ask if not provided)
3. **PPT descriptions** — one or more slide descriptions (the user's detailed layout text)
4. **Output directory** — where to save images (default: current working directory)

### Step 2: Select Slide Style

**Before generating any image**, ask the user to choose a style. Read `references/slide-styles.md` for full definitions. Present using AskUserQuestion:

| Code | Style Name | Best For |
|------|-----------|----------|
| `high-density` | 高信息密度咨询风格 | 高管汇报、方案深潜、交付物文档 |
| `medium-density` | 中信息密度咨询风格 | 项目状态、方案概览、干系人研讨会 |
| `executive` | 高管摘要风格 | 董事会汇报、投资提案、季度回顾 |
| `tech-arch` | 技术架构风格 | 架构设计、集成方案、技术规格书 |
| `digital` | 数字化转型风格 | 数字战略、转型路线图、创新提案 |
| `workshop` | 工作坊/培训风格 | 用户培训、工作坊引导、流程文档 |

The selected style's **prompt prefix** (from `references/slide-styles.md`) will be prepended to every slide prompt in this session. This ensures consistent visual language across all slides.

If the user has already specified a style in their description (e.g., "高信息密度"), skip the question and use that style directly.

### Step 3: Determine Generation Mode

- **Single slide** → generate directly in the current session
- **Multiple slides (2+)** → use **Agent parallel generation** (see Batch Generation section below)

### Step 4: Generate Image (Single Slide)

Call the API using curl:

```bash
curl -s -X POST "<API_ENDPOINT>" \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gemini-3.1-flash-image-preview-2K",
    "messages": [
      {
        "role": "user",
        "content": "<PROMPT>"
      }
    ],
    "max_tokens": 8192
  }'
```

### Step 5: Parse Response & Download

The API response JSON structure:
```json
{
  "choices": [{
    "message": {
      "content": "![image](<IMAGE_URL>)\n\n图片链接：<IMAGE_URL>"
    }
  }]
}
```

Extract the image URL from the response, then download:
```bash
curl -s -o <output_path> "<IMAGE_URL>"
```

### Step 6: Show Results

Use the Read tool to display each image to the user for review.

---

## Batch Generation (Agent Parallel Mode)

When the user provides **2 or more slides**, use Agent to generate all slides in parallel. Each image takes 30-120 seconds, so parallel execution dramatically reduces total wait time.

### Architecture

```
Main Session (you)
  ├── slide-gen-1  (Agent: generate slide 1)
  ├── slide-gen-2  (Agent: generate slide 2)
  ├── slide-gen-3  (Agent: generate slide 3)
  └── ...
```

### Step-by-step

#### 1. Create Tasks (one per slide)

For each slide, create a task via TaskCreate:
- **subject**: `Generate Slide N: <slide title>`
- **description**: Include the full curl command, API endpoint, API key, output path, and the complete prompt for that slide

#### 2. Spawn Parallel Agents

Launch one Agent per slide **in a single message** (this is critical — all Agent tool calls must be in the same message to run in parallel):

Each agent's prompt should be self-contained and include everything needed:

```
You are a PPT slide image generator. Execute the following steps:

1. Run this curl command to generate the image:

curl -s -X POST "<API_ENDPOINT>" \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '<JSON_PAYLOAD>' \
  -o <OUTPUT_DIR>/response_slideN.json

2. Extract the image URL from the response (look for URLs ending in .jpg/.png in the content field).

3. Download the image:
curl -s -o <OUTPUT_DIR>/slideN.jpg "$IMAGE_URL"

4. Verify the file exists and report its size.

5. If any step fails, retry once. Report success or failure.
```

**Important Agent parameters:**
- `description`: `Generate slide N`
- `run_in_background`: `true` (so all agents run concurrently)

#### 3. Monitor & Collect Results

As each agent completes (you'll be notified automatically):
- Check that the image file was downloaded successfully
- Use Read tool to display each completed image to the user
- Update the corresponding task status to `completed`

#### 4. Summarize

After all agents finish:
- Report total slides generated, file paths, any failures
- Display all images for final review

### Example: 5 Slides in Parallel

```
User provides: Slide 3, 4, 5, 6, 7 descriptions

→ TaskCreate × 5 (one per slide)
→ Agent × 5 in ONE message (all run_in_background=true)
   ├── slide-gen-3: curl → parse → download slide3.jpg
   ├── slide-gen-4: curl → parse → download slide4.jpg
   ├── slide-gen-5: curl → parse → download slide5.jpg
   ├── slide-gen-6: curl → parse → download slide6.jpg
   └── slide-gen-7: curl → parse → download slide7.jpg
→ Wait for notifications (each ~60-120s)
→ Display results as they arrive
→ Final summary
```

Total time: ~120s (parallel) vs ~600s (sequential). A 5x speedup.

---

## Brand Visual Guidelines

Brand visual definitions are stored in `references/westernacher-brand.md`. Read this file at the start of every generation session.

When constructing prompts, **always prepend the brand specifications** from the reference file before the user's slide content. This ensures consistent colors, fonts, icons, and layout rules across all generated slides.

The brand file defines:
- Color-to-element mapping: 章节标题=蓝色#1596D1, 主标题=深灰#3C4652, 正文=深灰#3C4652, 强调=蓝色#1596D1
- Secondary colors: grays (#282E36, #5D6A76, #858F98), blues (#72B7E2, #92CCEA), violets (#3E276D, #7768AE, #CECAE5), browns (#8B744D, #BDA06E, #D8C6A8)
- Typography: Microsoft YaHei, with specific sizes per element type
- Icon style: Line Icons, clean and minimal
- Color prohibitions: no colors outside brand palette, no color swatches/cards in output, no hex values as visible text
- Output restrictions: no percentage annotations, no font size labels, no section markers, no logo

## Input Preprocessing (Critical Step)

User input is often a "consulting content planning draft" — rich in strategy, analysis, and abstract visual descriptions. This is NOT suitable for direct image generation. You MUST preprocess the input before constructing the prompt.

### Preprocessing Rules

For each slide, process the user's raw input through these 3 stages:

#### Stage 1: Extract & Refine Page Text Content

From the user's input, identify all text that should actually appear on the slide (titles, bullet points, data callouts, labels, annotations). Then use the strategic context (e.g., "战略背景", "页面意图", "业务背景") to **refine and sharpen the body content**:

- **Titles (章节标题/主标题)**: Keep as-is from user input — do NOT modify
- **Body content (bullet points, descriptions, callouts)**: Use the strategic context to refine:
  - Use the stated intent/audience to ensure content is compelling and relevant
  - Use the business context to make bullet points more precise and impactful
  - Tighten verbose descriptions into concise slide-ready language
  - Keep domain-specific terms (SAP, BOM, KMAT, etc.) but remove explanatory prose that belongs in speaker notes, not on the slide
  - Preserve key data points and metrics (e.g., "80%", "100ms", "¥1,300") — these are high-impact visual elements

**Example transformation (body content only):**
- Raw input: "传统刀具制造企业面临'主数据灾难'——每个参数组合都需要独立SKU，导致系统内存在数十万僵尸物料"
- Refined: "每个参数组合需独立SKU → 数十万僵尸物料 → 维护成本高昂" (concise, visual-friendly)

#### Stage 2: Extract Layout & Visual Structure

From the "Visual Logic" section, extract concrete spatial information and convert abstract descriptions into specific layout instructions:

- **Layout type**: left-right split, top-bottom, center-radial, etc. — state as a clear spatial arrangement
- **Content zones**: what goes where (left panel content, right panel content, top bar, bottom summary)
- **Visual elements**: convert vague descriptions to concrete ones:
  - ❌ "传递清晰高效的视觉感受" → too abstract for image generation
  - ✅ "右侧面板使用蓝色(#1596D1)背景色块，包含树状结构图" → concrete and actionable
  - ❌ "动态箭头和高亮效果强调实时联动" → animation concepts, not static image
  - ✅ "中间放置蓝色粗箭头从左指向右，箭头上方标注文字'数字化转型'" → specific placement
- **Icons**: specify icon type and position (e.g., "齿轮图标放在流程节点2的位置") rather than vague references
- **Color assignments**: map to brand colors explicitly (use hex codes from brand guidelines)

#### Stage 3: Strip Non-Visual Content

Remove everything that does NOT contribute to the generated image:

- Strategic background and intent analysis ("战略背景与页面意图")
- Audience descriptions ("向决策层展示...")
- Business justification prose ("传统刀具制造企业面临...")
- Markdown formatting artifacts (###, **, ---, numbered sections)
- Animation or interaction descriptions ("动画效果暗示", "鼠标点击", "弹出气泡")
- Meta-commentary ("前3页已完成输出", "请回复继续")
- Speaker notes or presentation delivery guidance

### Preprocessed Output Format

After preprocessing, each slide should be restructured into this clean format before passing to the prompt template:

```
页面类型：内容页
页码：N

章节标题：<section name>
主标题：<refined action title>

布局：<layout type, e.g., 左右对比式>

左侧区域：
- <concrete visual element + position>
- <text content to display>

右侧区域：
- <concrete visual element + position>
- <text content to display>

底部区域：
- <summary bar / data callout>

关键视觉元素：
- <specific icon/arrow/highlight with position and color>
```

---

## Prompt Construction

After preprocessing, wrap the cleaned content into a generation prompt:

1. **Prepend brand block**: Read `references/westernacher-brand.md` and include the full brand specifications
2. **Add generation instruction**: "Generate a high-quality PPT slide image. White background, professional consulting style, 2K resolution."
3. **Include layout positions** — the model respects spatial instructions like coordinates and percentages
4. **Include preprocessed content** — use the cleaned, restructured slide description from the preprocessing step (NEVER pass raw user input directly)
5. **Append output restrictions**: No percentage/size annotations, no logo, strict positioning per the description

### Prompt Template

```
生成一张高质量PPT幻灯片图片。这是一张真实的商务咨询演示文稿页面，不是设计稿或色板展示。

## 绝对禁止（违反任何一条即为失败）

1. 禁止出现颜色色卡、色板、颜色样本、颜色方块展示
2. 禁止在画面中显示任何 hex 颜色值文字（如 #3C4652）
3. 禁止出现字体大小标签（如 24pt、12pt）
4. 禁止出现 section、top、bottom 等布局标注
5. 禁止出现 Westernacher logo
6. 禁止使用以下颜色以外的任何颜色

## 颜色规则（严格执行，不可偏离）

页面背景：纯白色
章节标题（页面左上角小字）：蓝色 #1596D1
主标题（章节标题下方大字加粗）：深灰色 #3C4652
所有正文文字：深灰色 #3C4652
强调数据/关键指标：蓝色 #1596D1
次要文字/分隔线：#5D6A76
弱化元素：#858F98
数据可视化/图表：#72B7E2 或 #92CCEA
页码（右下角8pt）：#858F98

## 风格要求

<STYLE PROMPT PREFIX — from references/slide-styles.md, based on user's chosen style>

## 排版要求

- 字体：微软雅黑 (Microsoft YaHei)
- 图标：简洁线性图标 (Line Icons)，细线条，颜色跟随所在区域
- 严格按照下方描述的位置和层级关系输出每个元素
- 页面必须看起来像一张真实的、可以直接用于客户汇报的PPT页面

## 详细内容排版

<PREPROCESSED SLIDE DESCRIPTION HERE>
```

**Example** (high-density style):
```
生成一张高质量PPT幻灯片图片。这是一张真实的商务咨询演示文稿页面，不是设计稿或色板展示。

## 绝对禁止（违反任何一条即为失败）

1. 禁止出现颜色色卡、色板、颜色样本、颜色方块展示
2. 禁止在画面中显示任何 hex 颜色值文字（如 #3C4652）
3. 禁止出现字体大小标签（如 24pt、12pt）
4. 禁止出现 section、top、bottom 等布局标注
5. 禁止出现 Westernacher logo
6. 禁止使用以下颜色以外的任何颜色

## 颜色规则（严格执行，不可偏离）

页面背景：纯白色
章节标题（页面左上角小字）：蓝色 #1596D1
主标题（章节标题下方大字加粗）：深灰色 #3C4652
所有正文文字：深灰色 #3C4652
强调数据/关键指标：蓝色 #1596D1
次要文字/分隔线：#5D6A76
弱化元素：#858F98
数据可视化/图表：#72B7E2 或 #92CCEA
页码（右下角8pt）：#858F98

## 风格要求

高信息密度咨询风格。布局紧凑，多栏排版，内容区块丰富，每张幻灯片承载大量信息。适合高管和领域专家阅读。字体偏小(10-12pt正文)，充分利用页面空间。包含对比表格、流程图、数据卡片等多种信息组件。页面上的每一寸空间都应承载有意义的信息，最小化留白。

## 排版要求

- 字体：微软雅黑 (Microsoft YaHei)
- 图标：简洁线性图标 (Line Icons)，细线条
- 严格按照下方描述的位置和层级关系输出每个元素
- 页面必须看起来像一张真实的、可以直接用于客户汇报的PPT页面

## 详细内容排版
...（预处理后的幻灯片描述）
```

## Error Handling

- **401 Unauthorized**: API key is invalid or expired. Ask the user to verify their key.
- **Timeout (>3 min)**: The generation may have failed. Retry once, then report the issue.
- **Empty image URL**: The model may have returned text-only. Retry with a more explicit image generation instruction.
- **Response has no choices**: Check if the model name is correct or if there's a rate limit.
- **Agent failure**: If an agent fails, retry by spawning a replacement agent for that specific slide only.

## Example Usage

User says: "I have 5 slides for my consulting deck, here are the descriptions, API endpoint is https://xxx, API key is sk-xxx..."

Response flow:
1. Confirm: "I'll generate 5 slide images in parallel. Output to [directory]. Proceed?"
2. Create tasks
3. Spawn 5 agents in parallel (one per slide)
4. As each completes, display the image
5. Final summary with all file paths
