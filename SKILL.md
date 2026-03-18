---
name: Westernacher-PPT
description: "Generate PPT slide images using the Nanabanana API (gemini-3.1-flash-image-preview). Use this skill whenever the user asks to generate PPT images, create slide visuals, make presentation graphics, or mentions Nano Banana Pro/Nano Banana. Also trigger when the user provides detailed PPT layout descriptions and wants them turned into actual images. Covers single and batch (parallel Agent Team) generation of consulting-style PPT slides."
---

# Westernacher PPT Image Generator

Generate high-quality PPT slide images from detailed text descriptions using the Nano Banana API (OpenAI-compatible endpoint with Gemini image generation models).

## API Configuration

- **Endpoint**: `https://api.chr1.com/v1/chat/completions`
- **Model**: `gemini-3.1-flash-image-preview-2K` (default, 2K resolution)
- **Alternative models**: `gemini-3.1-flash-image-preview` (standard), `gemini-3.1-flash-image-preview-4K` (4K)
- **API Key**: Must be provided by the user at runtime. At the start of the conversation, if the user hasn't provided a key, ask: "Please provide your Nanabanana API Key (format: sk-xxx)"

## How It Works

The API is OpenAI chat completions compatible. Send the PPT description as a user message, and the model returns a markdown image link with the generated image URL. Download the image to save it locally.

## Workflow

### Step 1: Collect Inputs

Gather from the user:
1. **API Key** (required, ask if not provided)
2. **PPT descriptions** — one or more slide descriptions (the user's detailed layout text)
3. **Output directory** — where to save images (default: current working directory)

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
- **Multiple slides (2+)** → use **Agent Team parallel generation** (see Batch Generation section below)

### Step 3: Generate Image (Single Slide)

Call the API using curl:

```bash
curl -s -X POST "https://api.chr1.com/v1/chat/completions" \
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

### Step 4: Parse Response & Download

The API response JSON structure:
```json
{
  "choices": [{
    "message": {
      "content": "![image](https://image.chr1.com/images/<uuid>.jpg)\n\n图片链接：https://image.chr1.com/images/<uuid>.jpg"
    }
  }]
}
```

Extract the image URL (grep for `https://image.chr1.com/images/...`), then download:
```bash
curl -s -o <output_path> "<image_url>"
```

### Step 5: Show Results

Use the Read tool to display each image to the user for review.

---

## Batch Generation (Agent Team Parallel Mode)

When the user provides **2 or more slides**, use Agent Team to generate all slides in parallel. Each image takes 30-120 seconds, so parallel execution dramatically reduces total wait time.

### Architecture

```
Team Lead (you)
  ├── slide-gen-1  (Agent: generate slide 1)
  ├── slide-gen-2  (Agent: generate slide 2)
  ├── slide-gen-3  (Agent: generate slide 3)
  └── ...
```

### Step-by-step

#### 1. Create the Team

```
TeamCreate: team_name="ppt-gen", description="Batch PPT image generation"
```

#### 2. Create Tasks (one per slide)

For each slide, create a task via TaskCreate:
- **subject**: `Generate Slide N: <slide title>`
- **description**: Include the full curl command, API key, output path, and the complete prompt for that slide

#### 3. Spawn Parallel Agents

Launch one Agent per slide **in a single message** (this is critical — all Agent tool calls must be in the same message to run in parallel):

Each agent's prompt should be self-contained and include everything needed:

```
You are a PPT slide image generator. Execute the following steps:

1. Run this curl command to generate the image:

curl -s -X POST "https://api.chr1.com/v1/chat/completions" \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '<JSON_PAYLOAD>' \
  -o <OUTPUT_DIR>/response_slideN.json

2. Extract the image URL from the response:
IMAGE_URL=$(grep -oP 'https://image\.chr1\.com/images/[^\"\)\\s]+' <OUTPUT_DIR>/response_slideN.json | head -1)

3. Download the image:
curl -s -o <OUTPUT_DIR>/slideN.jpg "$IMAGE_URL"

4. Verify the file exists and report its size.

5. If any step fails, retry once. Report success or failure.
```

**Important Agent parameters:**
- `name`: `slide-gen-N` (for addressing)
- `team_name`: `ppt-gen`
- `run_in_background`: `true` (so all agents run concurrently)
- `description`: `Generate slide N`

#### 4. Monitor & Collect Results

As each agent completes (you'll be notified automatically):
- Check that the image file was downloaded successfully
- Use Read tool to display each completed image to the user
- Update the corresponding task status to `completed`

#### 5. Summarize

After all agents finish:
- Report total slides generated, file paths, any failures
- Display all images for final review
- Clean up: `TeamDelete` to remove the team

### Example: 5 Slides in Parallel

```
User provides: Slide 3, 4, 5, 6, 7 descriptions

→ TeamCreate("ppt-gen")
→ TaskCreate × 5 (one per slide)
→ Agent × 5 in ONE message (all run_in_background=true)
   ├── slide-gen-3: curl → parse → download slide3.jpg
   ├── slide-gen-4: curl → parse → download slide4.jpg
   ├── slide-gen-5: curl → parse → download slide5.jpg
   ├── slide-gen-6: curl → parse → download slide6.jpg
   └── slide-gen-7: curl → parse → download slide7.jpg
→ Wait for notifications (each ~60-120s)
→ Display results as they arrive
→ Final summary + TeamDelete
```

Total time: ~120s (parallel) vs ~600s (sequential). A 5x speedup.

---

## Brand Visual Guidelines

Brand visual definitions are stored in `references/westernacher-brand.md`. Read this file at the start of every generation session.

When constructing prompts, **always prepend the brand specifications** from the reference file before the user's slide content. This ensures consistent colors, fonts, icons, and layout rules across all generated slides.

The brand file defines:
- Primary colors: Dark Gray #3C4652, White #FFFFFF, Blue #1596D1
- Secondary colors: grays (#5D6A76, #858F98) and blues (#72B7E2, #92CCEA)
- Typography: Microsoft YaHei, with specific sizes per element type
- Icon style: Line Icons, clean and minimal
- Output restrictions: no percentage annotations, no font size labels, no section markers, no logo

## Prompt Construction

When the user provides raw PPT content descriptions, wrap them into a complete generation prompt:

1. **Prepend brand block**: Read `references/westernacher-brand.md` and include the full brand specifications
2. **Add generation instruction**: "Generate a high-quality PPT slide image. White background, professional consulting style, 2K resolution."
3. **Include layout positions** — the model respects spatial instructions like coordinates and percentages
4. **Include content verbatim** — don't summarize the user's text content; pass it through exactly as written
5. **Append output restrictions**: No percentage/size annotations, no logo, strict positioning per the description

### Prompt Template

```
根据以下PPT描述文本生成一张PPT幻灯片图片。白底商务咨询风格，2K清晰度。

## 风格要求

<STYLE PROMPT PREFIX — from references/slide-styles.md, based on user's chosen style>

## Westernacher 品牌视觉标准

### 主色调
Dark Gray #3C4652 | White #FFFFFF | Blue #1596D1

### 次要颜色
灰色系：#5D6A76 (中灰) | #858F98 (浅灰)
蓝色系：#72B7E2 (中蓝) | #92CCEA (淡蓝)

### 排版要求
- 字体：微软雅黑 (Microsoft YaHei)
- 图标：简洁的 Line Icons
- 限制：不要在图片中输出包含任何关于百分比、文字大小(如24pt、12pt等)、section、top等标注描述
- 页脚：右下角放置8号字体的页码
- 无需输出Westernacher logo
- 严格按照位置以及文字大小定义输出

## 详细内容排版

<USER'S SLIDE DESCRIPTION HERE>
```

**Example** (high-density style):
```
根据以下PPT描述文本生成一张PPT幻灯片图片。白底商务咨询风格，2K清晰度。

## 风格要求

高信息密度咨询风格。布局紧凑，多栏排版，内容区块丰富，每张幻灯片承载大量信息。适合高管和领域专家阅读。字体偏小(10-12pt正文)，充分利用页面空间。包含对比表格、流程图、数据卡片等多种信息组件。

## Westernacher 品牌视觉标准
...（品牌规范）

## 详细内容排版
...（用户的幻灯片描述）
```

## Error Handling

- **401 Unauthorized**: API key is invalid or expired. Ask the user to verify their key.
- **Timeout (>3 min)**: The generation may have failed. Retry once, then report the issue.
- **Empty image URL**: The model may have returned text-only. Retry with a more explicit image generation instruction.
- **Response has no choices**: Check if the model name is correct or if there's a rate limit.
- **Agent failure**: If an agent fails, retry by spawning a replacement agent for that specific slide only.

## Example Usage

User says: "I have 5 slides for my consulting deck, here are the descriptions, API key is sk-xxx..."

Response flow:
1. Confirm: "I'll generate 5 slide images in parallel using Agent Team. Output to [directory]. Proceed?"
2. Create team + tasks
3. Spawn 5 agents in parallel (one per slide)
4. As each completes, display the image
5. Final summary with all file paths
6. Clean up team
