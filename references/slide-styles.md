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
- Balanced layouts with 1-2 main content zones per slide
- Moderate font sizes (12-14pt body), readable at projection distance
- Key message highlighted at top, supporting details below
- Icon + text combinations for key points
- Comfortable whitespace between sections
- One primary visual element (chart, diagram, or flow) per slide
- Clean dividers and section headers

**When to use**: Project status updates, solution overview presentations, stakeholder workshops, phase gate reviews

**Prompt prefix**:
> 中信息密度咨询风格。布局平衡，1-2个主要内容区域，正文字体12-14pt。顶部突出核心观点，下方展开支撑细节。图标+文字组合呈现要点，留有适度留白。每页一个主要可视化元素（图表/流程图/架构图）。

---

## Style 3: Executive Summary (高管摘要风格)

**Code**: `executive`
**Target**: C-level executives, board members, investment committees
**Audience expectation**: 10 seconds per slide — instant comprehension required

**Visual characteristics**:
- One core message per slide, large prominent headline (24-28pt)
- Maximum 3 bullet points or data callouts
- Large KPI numbers with trend indicators (arrows, color coding)
- Generous whitespace (40%+ of page area)
- Bold color blocks for emphasis, minimal decorative elements
- Dashboard-style metrics: big numbers + small labels
- No footnotes or technical jargon on the main canvas

**When to use**: Board presentations, investment proposals, quarterly business reviews, elevator pitches

**Prompt prefix**:
> 高管摘要风格。每页只传递一个核心信息，大标题(24-28pt)醒目突出。最多3个要点或数据亮点。大号KPI数字配趋势指示。大量留白(页面40%以上)，极简装饰。仪表盘式指标呈现：大数字+小标签。无技术脚注。

---

## Style 4: Technical Architecture (技术架构风格)

**Code**: `tech-arch`
**Target**: IT architects, technical leads, development teams
**Audience expectation**: Precise technical communication with system relationships

**Visual characteristics**:
- System/component boxes with clear boundaries and labels
- Directional arrows showing data flow, integration paths, API calls
- Layer diagrams (presentation → business logic → data → infrastructure)
- Color-coded by system/module (e.g., SAP blue, custom green, third-party gray)
- Protocol/technology labels on connection lines (REST, RFC, IDoc, OData)
- Legend/key for color and icon meanings
- Monospace font for technical identifiers (table names, endpoints, transaction codes)

**When to use**: Solution architecture documents, integration design, system landscape overviews, technical specifications

**Prompt prefix**:
> 技术架构风格。以系统组件框图为核心，清晰的边界和标签。方向箭头展示数据流、集成路径和API调用。分层架构图（展示层→业务逻辑层→数据层→基础设施层）。按系统/模块色彩编码。连接线标注协议/技术(REST, RFC, IDoc, OData)。技术标识符使用等宽字体。包含图例说明。

---

## Style 5: Digital Transformation (数字化转型风格)

**Code**: `digital`
**Target**: Business + IT mixed audience, transformation steering groups
**Audience expectation**: Inspiring yet grounded — vision meets execution

**Visual characteristics**:
- Modern, clean aesthetic with gradient accents
- Timeline/roadmap as primary visual metaphor
- Before → After or Current → Future state comparisons
- Maturity model stages with progress indicators
- Icon-heavy with minimal text (icon speaks louder than words)
- Subtle background patterns (dots, lines) suggesting technology
- Value proposition callouts with benefit metrics

**When to use**: Digital strategy presentations, transformation roadmaps, innovation proposals, change management decks

**Prompt prefix**:
> 数字化转型风格。现代简洁美学，带渐变色点缀。时间轴/路线图为主要视觉隐喻。现状→未来状态对比。成熟度模型分阶段展示，配进度指示。图标为主、文字精简。微妙的科技感背景纹理（点阵、线条）。价值主张配量化收益指标。

---

## Style 6: Workshop & Training (工作坊/培训风格)

**Code**: `workshop`
**Target**: End users, workshop participants, training attendees
**Audience expectation**: Step-by-step guidance, easy to follow along

**Visual characteristics**:
- Large fonts (14-16pt body), high readability
- Numbered steps with visual progression (step 1 → 2 → 3)
- Screenshot placeholders or system UI mockups
- Callout boxes for tips, warnings, and best practices
- Checklists with checkmarks
- Friendly color palette — softer tones, less corporate intensity
- "Try it yourself" or "Hands-on" section markers

**When to use**: User training materials, workshop facilitation decks, onboarding guides, process documentation

**Prompt prefix**:
> 工作坊/培训风格。大字体(14-16pt正文)，高可读性。编号步骤配视觉递进(步骤1→2→3)。系统界面示意图。提示框标注注意事项和最佳实践。清单配勾选标记。柔和友好的配色，降低企业严肃感。包含"动手实操"区域标记。
