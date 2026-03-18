# PPT Slide Styles

When generating slides, the user must choose a style. Each style defines information density, visual language, and target audience. Always ask the user to select a style before generation.

---

## Style 1: High-Density Consulting (高信息密度咨询风格)

**Code**: `high-density`
**Target**: Senior management, steering committee, project sponsors
**Audience expectation**: Readers are domain experts who want maximum information per slide

**Visual characteristics**:
- Information-packed layouts with 4-6 distinct content zones per slide
- Very small font sizes (8-10pt body, 7-8pt footnotes), extremely dense text blocks
- Multi-column layouts (2-3 columns), nested cards, tables, and sub-sections within each column
- Multiple hierarchy levels: section header → sub-headers → bullet lists → nested sub-bullets → footnotes
- Horizontal process flow diagrams spanning the full page width with icons and labels at each step
- Detail callout boxes with technical specifications (e.g., SAP transaction codes, configuration parameters, region codes)
- Comparison tables, before/after contrasts, checklists with checkmarks
- Minimal whitespace — page utilization rate should be 85-90%, almost no empty space
- Color-coded sections using brand palette to differentiate content zones
- Bottom section with compliance notes, validation mechanisms, or summary footnotes
- The page should feel like a McKinsey/BCG deliverable — packed with structured, actionable information

**When to use**: Strategy presentations, solution architecture deep-dives, project deliverable decks, due diligence reports

**Prompt prefix**:
> 高信息密度咨询风格（参考麦肯锡/BCG交付物标准）。页面信息极度密集，包含4-6个独立内容区块。正文字体8-10pt，脚注7-8pt。多栏排版（2-3栏），每栏内部还有嵌套的子区块、表格和列表。必须包含：多层级标题体系（章节→子标题→要点→子要点→脚注）、横跨页面全宽的流程图（带图标和步骤标签）、技术细节标注框（含SAP事务码、配置参数等）、底部合规说明或验证机制区域。页面利用率85-90%，几乎没有空白区域。每一寸空间都必须承载有意义的结构化信息。这是一张可以直接交付给客户高管的咨询文档页面。

---

## Style 2: Medium-Density Consulting (中信息密度咨询风格)

**Code**: `medium-density`
**Target**: Department heads, project managers, business stakeholders
**Audience expectation**: Readers want clear takeaways with supporting detail

**Visual characteristics**:
- Balanced layouts with 2-3 distinct content zones per slide
- Moderate font sizes (10-12pt body, 8-9pt footnotes)
- Top 20% of page: chapter title + action title (key message), visually separated by a thin blue #1596D1 horizontal line
- Middle 60%: 2-column layout, each column contains a content card with header bar, bullet list, and optional icon/diagram
- Bottom 20%: summary bar or key takeaway strip with 2-3 metrics side by side
- Each content card has a colored header bar (blue #1596D1 or dark gray #3C4652) with white text title
- Bullet points use "·" markers, with 1 level of sub-bullets indented
- One primary visual element per slide: a simple flow diagram, comparison table, or 2x2 matrix
- Icon + text combinations for key points, icons are 24x24px line style
- Clean dividers (1px gray #858F98 lines) between sections
- Page utilization rate 65-75%, with intentional whitespace for readability at projection distance
- Footnotes area at bottom with SAP transaction codes or source references in 8pt gray #858F98

**When to use**: Project status updates, solution overview presentations, stakeholder workshops, phase gate reviews

**Prompt prefix**:
> 中信息密度咨询风格。页面包含2-3个内容区块，正文字体10-12pt，脚注8-9pt。顶部20%区域放置章节标题和主标题，用蓝色细横线分隔。中部60%采用双栏布局，每栏包含带彩色标题栏的内容卡片（蓝色或深灰背景白字标题），卡片内为要点列表和可选图标/图表。底部20%为汇总条，并排展示2-3个关键指标。每页包含一个主要可视化元素（流程图/对比表/矩阵图）。页面利用率65-75%，保留适度留白确保投影可读性。底部脚注区域用8pt灰色字体标注SAP事务码或数据来源。

---

## Style 3: Executive Summary (高管摘要风格)

**Code**: `executive`
**Target**: C-level executives, board members, investment committees
**Audience expectation**: 10 seconds per slide — instant comprehension required

**Visual characteristics**:
- Top 15%: chapter title (blue #1596D1, 10pt) + action title (dark gray #3C4652, 22-26pt bold) — the action title IS the core message, must be a complete sentence
- Middle 50%: maximum 3 large KPI cards arranged horizontally, each card contains:
  - A large number (36-48pt, blue #1596D1 bold)
  - A trend indicator arrow (up/down, green/red)
  - A short label below (10pt, gray #5D6A76)
  - Optional: a thin sparkline or mini bar chart inside the card
- Bottom 25%: one single insight sentence or a 3-column comparison strip (Before → Now → Target)
- Generous whitespace — page utilization rate 40-55%, white space is a design element
- No bullet point lists — information is conveyed through KPI cards, numbers, and visual indicators
- Color blocks: use blue #1596D1 or dark gray #3C4652 as card backgrounds sparingly (max 1-2 cards)
- No footnotes, no technical jargon, no SAP transaction codes on the visible page
- Typography hierarchy is extreme: title is 22-26pt, KPI numbers are 36-48pt, everything else is 10pt or smaller
- The page should feel like a Bloomberg terminal dashboard or a CFO's one-page summary

**When to use**: Board presentations, investment proposals, quarterly business reviews, elevator pitches

**Prompt prefix**:
> 高管摘要风格（参考Bloomberg仪表盘/CFO单页摘要标准）。顶部15%放置章节标题和主标题（主标题22-26pt加粗，必须是完整的结论性语句）。中部50%最多放置3个大型KPI卡片横向排列，每个卡片包含：大号数字（36-48pt蓝色加粗）、趋势箭头（上升绿色/下降红色）、底部小标签（10pt灰色）。底部25%放置一句洞察总结或三栏对比条（过去→现在→目标）。页面利用率40-55%，大量留白是设计元素。禁止使用要点列表，信息通过KPI卡片和数字传递。禁止出现脚注、技术术语或SAP事务码。字体层级极端：标题22-26pt，KPI数字36-48pt，其余10pt以下。

---

## Style 4: Technical Architecture (技术架构风格)

**Code**: `tech-arch`
**Target**: IT architects, technical leads, development teams
**Audience expectation**: Precise technical communication with system relationships

**Visual characteristics**:
- Top 10%: chapter title + action title (concise technical statement)
- Main area 75%: architecture diagram as the central element, structured as:
  - System/component boxes: rounded rectangles with 2px borders, each box contains system name (bold) + brief description (regular)
  - Boxes are color-coded by domain: SAP systems = blue #1596D1, custom/middleware = mid blue #72B7E2, external/third-party = gray #858F98, databases = dark gray #3C4652
  - Directional arrows (1.5px solid lines) connecting boxes, with protocol labels on each arrow (REST, RFC, IDoc, OData, BAPI, SOAP)
  - Layer separation: horizontal dashed lines divide the diagram into layers (Presentation → Business Logic → Integration → Data → Infrastructure), each layer labeled on the left margin
  - Swim lanes for different organizational domains if applicable
- Right margin or bottom 15%: legend/key box explaining color codes and icon meanings
- Technical identifiers (table names, endpoints, transaction codes like SE38, SM59, SPRO) displayed in monospace-style font
- Data flow direction clearly indicated with arrow heads, numbered if sequential
- Optional: small callout boxes attached to specific components with configuration details (e.g., "RFC Destination: SAP_ECC_100", "OData Service: /sap/opu/odata/...")
- Page utilization rate 75-85%, diagram should fill the available space
- No decorative elements — every visual element carries technical meaning

**When to use**: Solution architecture documents, integration design, system landscape overviews, technical specifications

**Prompt prefix**:
> 技术架构风格（参考SAP Solution Manager/Sparx EA架构图标准）。顶部10%放置章节标题和主标题（简洁技术陈述）。主区域75%为架构图：系统组件用圆角矩形框表示（2px边框），框内包含系统名（加粗）+简述。按域色彩编码：SAP系统=蓝色#1596D1，中间件=中蓝#72B7E2，第三方=灰色#858F98，数据库=深灰#3C4652。连接箭头（1.5px实线）标注协议（REST/RFC/IDoc/OData/BAPI）。水平虚线分隔架构层级（展示层→业务逻辑层→集成层→数据层→基础设施层），左侧标注层名。右下角图例框说明颜色和图标含义。技术标识符（事务码SE38/SM59/SPRO、表名、端点）使用等宽风格字体。可选：组件旁附加配置详情标注框。页面利用率75-85%，无装饰元素，每个视觉元素都承载技术含义。

---

## Style 5: Digital Transformation (数字化转型风格)

**Code**: `digital`
**Target**: Business + IT mixed audience, transformation steering groups
**Audience expectation**: Inspiring yet grounded — vision meets execution

**Visual characteristics**:
- Top 15%: chapter title + action title (aspirational yet specific, e.g., "从手工报价到智能配置：3个月实现端到端数字化")
- Main area structured as a horizontal timeline/roadmap (left to right):
  - 3-5 phase blocks arranged horizontally, each phase is a rounded rectangle card
  - Each phase card contains: phase name (bold header), 2-3 key deliverables (bullet list), a maturity indicator (filled/unfilled circles or progress bar)
  - Phase cards connected by thick gradient arrows (transitioning from gray #858F98 on the left to blue #1596D1 on the right, symbolizing progression)
  - Current phase highlighted with blue #1596D1 border and subtle blue #92CCEA background tint
  - Completed phases have a checkmark overlay, future phases have a dotted border
- Above the timeline: benefit metrics floating as callout badges (e.g., "效率+80%", "成本-30%"), each badge is a small rounded pill with blue #1596D1 background and white text
- Below the timeline: enabling technologies or prerequisites listed as small icon+label pairs (e.g., cloud icon + "BTP", gear icon + "AVC", shield icon + "GDPR")
- Bottom 10%: a "Current State → Future State" comparison strip with 2-3 before/after pairs
- Subtle background texture: very light gray (#F5F6F7) dot grid pattern covering the main area, suggesting digital/tech context
- Page utilization rate 70-80%, whitespace is used strategically to create visual breathing room between phases
- Icons are prominent (32x32px), used as visual anchors for each phase or technology
- No dense text blocks — maximum 3 lines per content zone

**When to use**: Digital strategy presentations, transformation roadmaps, innovation proposals, change management decks

**Prompt prefix**:
> 数字化转型风格（参考Gartner/Forrester路线图标准）。顶部15%放置章节标题和主标题（既有愿景又具体）。主区域为水平时间轴/路线图：3-5个阶段卡片横向排列，每个卡片包含阶段名（加粗标题）、2-3个关键交付物、成熟度指示器（实心/空心圆点或进度条）。阶段间用粗渐变箭头连接（从灰色#858F98渐变到蓝色#1596D1，象征进步）。当前阶段用蓝色#1596D1边框+浅蓝#92CCEA底色高亮，已完成阶段叠加勾选标记，未来阶段用虚线边框。时间轴上方：收益指标浮动徽章（蓝色#1596D1背景白字药丸形状，如"效率+80%"）。时间轴下方：使能技术用小图标+标签对排列。底部10%：现状→未来对比条。背景使用极浅灰(#F5F6F7)点阵纹理。页面利用率70-80%，图标突出（32x32px），每个内容区最多3行文字。

---

## Style 6: Workshop & Training (工作坊/培训风格)

**Code**: `workshop`
**Target**: End users, workshop participants, training attendees
**Audience expectation**: Step-by-step guidance, easy to follow along

**Visual characteristics**:
- Top 15%: chapter title + action title (instructional tone, e.g., "如何在SAP中创建可配置物料：分步操作指南")
- Main area structured as numbered steps (top to bottom or left to right):
  - 3-4 step cards, each card contains:
    - Step number in a large blue #1596D1 circle (28pt bold white text)
    - Step title (12pt bold dark gray #3C4652)
    - 2-3 instruction lines (12pt regular dark gray #3C4652)
    - A system UI mockup or screenshot placeholder (rounded rectangle with light gray #858F98 border, labeled "SAP Fiori界面" or similar)
  - Steps connected by vertical or horizontal arrows (blue #1596D1, 2px)
- Right sidebar or inline callout boxes (3 types, color-coded):
  - Tip box: light blue #92CCEA background, blue #1596D1 left border (4px), lightbulb icon, "提示：" prefix
  - Warning box: light brown #D8C6A8 background, brown #8B744D left border (4px), warning triangle icon, "注意：" prefix
  - Best practice box: light violet #CECAE5 background, violet #7768AE left border (4px), star icon, "最佳实践：" prefix
- Bottom 15%: checklist summary with 3-5 items, each with an empty checkbox "☐" and action text
- Font sizes are larger than other styles: 12-14pt body, 10pt callout text, 14pt step titles
- Page utilization rate 60-70%, generous spacing between steps for readability
- Friendly visual tone: rounded corners (8px radius) on all cards and boxes, softer color palette using secondary brand colors (light blue, light violet, light brown)
- Icons are friendly and illustrative (not abstract), 28x28px

**When to use**: User training materials, workshop facilitation decks, onboarding guides, process documentation

**Prompt prefix**:
> 工作坊/培训风格（参考SAP Enable Now/WalkMe操作指南标准）。顶部15%放置章节标题和主标题（教学语气）。主区域为编号步骤卡片（3-4步），每步包含：蓝色#1596D1大圆圈步骤编号（28pt白色加粗）、步骤标题（12pt加粗）、2-3行操作说明（12pt）、系统界面示意图占位框。步骤间用蓝色箭头连接。侧边或内嵌3种标注框：提示框（浅蓝#92CCEA底+蓝色左边框+灯泡图标）、警告框（浅棕#D8C6A8底+棕色左边框+警告图标）、最佳实践框（浅紫#CECAE5底+紫色左边框+星形图标）。底部15%为清单摘要（3-5项，空心复选框+操作文字）。字体偏大：正文12-14pt，标注10pt，步骤标题14pt。页面利用率60-70%，步骤间留有充足间距。所有卡片和框使用8px圆角，配色使用品牌次要色（浅蓝、浅紫、浅棕）营造友好氛围。
