---
name: hjz-ui-rule-maker
description: 根据提供的 UI 设计方案文档（Markdown/SCSS），分析并更新项目的 UI 规范 Skill (hjz-ui-rule) 及全局样式配置。优先复用 Uniapp 内置变量及 uv-ui 变量，实现主题自动适配。
---

# HongJuZi UI Rule Maker Skill

此 Skill 用于将设计师提供的静态 UI 规范文档（如 `ui-design-spec.md` 和 `variables.scss`）转化为 Trae IDE 可执行的标准化 Skill (`hjz-ui-rule`)，从而在后续开发中强制应用统一的设计规范。

**重要原则：优先更新 Uniapp 标准变量 (`uni.scss` 中的内置变量) 及 uv-ui 变量，仅在无法映射时才创建 `$hjz-` 前缀的新变量。** 这样可以确保 Uniapp 的默认组件和 uv-ui 组件自动适配新主题。

## 1. 核心任务流程

当用户调用此 Skill 并提供包含 UI 设计资料的目录路径时，执行以下步骤：

### 1.1 分析阶段 (Analyze)
1.  **读取文件**：读取指定目录下的所有 `.md`（设计规范文档）和 `.scss`（变量定义）文件。
2.  **提取并映射 Design Tokens**：
    *   **色彩**：提取主色并映射到 `$uni-color-primary` / `$uv-primary`；提取辅助色映射到 `$uni-color-success`, `$uni-color-warning` 等。
    *   **背景**：提取页面背景映射到 `$uni-bg-color-grey` 或 `$uni-bg-color`。
    *   **排版**：提取字号映射到 `$uni-font-size-base`, `$uni-font-size-lg` 等。
    *   **形状**：提取圆角映射到 `$uni-border-radius-lg` 等。
    *   **无法映射的 Token**：仅对无法对应到标准变量的特殊设计 Token（如特殊的卡片阴影、特殊的渐变色）使用 `$hjz-` 前缀。

### 1.2 生成阶段 (Generate)

基于提取的 Token，生成以下两部分内容：

#### A. 项目专属 Skill (`hjz-ui-rule`)
在项目的 `.trae/skills/hjz-ui-rule/SKILL.md` 生成一个新的 Skill 文件（如果已存在则覆盖）。
该 Skill **必须严格包含以下标准结构和内容**（请将 `{{变量值}}` 替换为实际提取的值或对应的标准变量名）：

````markdown
---
name: hjz-ui-rule
description: 强制应用当前项目的统一 UI 设计规范，适用于所有业务模块的页面开发。包含色彩、字体、布局、组件等核心设计系统。
---

# HongJuZi UI Rule Skill

此 Skill 旨在确保项目所有模块的 UI 页面严格遵循统一的设计规范。当开发者需要创建新页面或重构旧页面时，必须调用此 Skill。

## 1. 核心设计原则 (Design Principles)

- **全局优先 (Global First)**: 优先使用 `uni.scss` 中定义的 Uniapp 标准变量 (`$uni-`, `$uv-`)，严禁在页面内重复定义通用样式变量。
- **统一性 (Consistency)**: 所有页面必须使用预定义的色彩系统、字体层级和间距规则。
- **卡片式 (Card-based)**: 内容展示以白色卡片为基础，背景统一为浅灰色 (`$uni-bg-color-grey`)。
- **圆润感 (Rounded)**: 强调圆角 (`$uni-border-radius-lg`)，营造亲和力。

## 2. 全局样式配置 (Global Configuration)

在使用本规范前，请确保项目的 `uni.scss` 文件中已包含以下核心变量定义（优先复用标准变量）：

| 类别 | 变量名 (SCSS) | 色值 | 用途说明 |
| :--- | :--- | :--- | :--- |
| **主色** | `$uni-color-primary` / `$uv-primary` | `{{$primary-color}}` | 核心按钮、选中状态 |
| **渐变起始** | `$uv-primary-gradient-start` | `{{$gradient-start}}` | 顶部背景渐变起始色 |
| **渐变结束** | `$uv-primary-gradient-end` | `{{$gradient-end}}` | 顶部背景渐变结束色 |
| **页面背景** | `$uni-bg-color-grey` | `{{$bg-page}}` | 整体页面底色 |
| **卡片背景** | `$uni-bg-color` | `{{$bg-card}}` | 内容卡片背景 |
| **圆角** | `$uni-border-radius-lg` | `{{$radius-card}}` | 卡片圆角 |

**混合宏 (Mixins)**:
- `@include hjz-card;`: 应用标准的卡片样式（使用标准变量组合）。
- `@include hjz-btn-primary;`: 应用标准的主色渐变按钮样式。

## 3. 设计规范详情 (Design Specs)

### 3.1 字体规范 (Typography)
- **特大标题**: `$uni-font-size-xxl` (Banner用)
- **大标题**: `$uni-font-size-xl` (模块标题)
- **中标题**: `$uni-font-size-lg` (列表项标题)
- **正文**: `$uni-font-size-base` (详细描述)
- **小字**: `$uni-font-size-sm` (辅助说明)

### 3.2 布局与间距
- **页面边距**: `$uni-spacing-col-lg` ({{$spacing-page}})
- **卡片间距**: `$uni-spacing-card` ({{$spacing-card}})

## 4. 代码生成指南 (Coding Guidelines)

### 4.1 Vue 3 + Setup 语法
- 必须使用 `<script setup>` 语法。
- 必须使用 `uni-app` 的内置组件 (`view`, `text`, `image`, `scroll-view`) 或 `uv-ui` 组件 (`uv-icon`, `uv-button`, `uv-cell`, `uv-avatar`, `uv-grid`)。
- **严禁** 使用 HTML 标签如 `div`, `span`, `img`。

### 4.2 SCSS 编写规范 (High Reuse)
- **引用全局变量**: 在 `<style lang="scss" scoped>` 中直接使用 `uni.scss` 中的变量（如 `$uni-color-primary`），**严禁** 在局部重新定义这些变量。
- **使用 Mixins**: 对于卡片容器，直接使用 `@include hjz-card;`。
- **单位**: 统一使用 `rpx`。

### 4.3 导航栏处理 (Navigation Bar)
- **Default (默认)**: 如果 `pages.json` 中该页面配置为默认导航栏（未设置 `navigationStyle: "custom"`），则 **不需要** 在 template 中编写顶部 Header 区域，仅需设置 `pages.json` 中的 `navigationBarTitleText`。
- **Custom (自定义)**: 如果 `pages.json` 中配置了 `navigationStyle: "custom"`，则 **必须** 在 template 顶部编写自定义 Header（通常包含背景渐变、搜索框等），并注意处理状态栏高度（使用 `systemInfo.statusBarHeight` 或占位）。

### 4.4 模板结构示例 (Template Structure)

**场景 A: 默认导航栏 (常规列表/详情页)**

```html
<template>
  <view class="page-container">
    <!-- 搜索区 (可选) -->
    <view class="search-section">...</view>

    <!-- 内容区域 -->
    <view class="content-section">
      <view class="card-item" v-for="(item, index) in list" :key="index">
        <!-- 卡片内容 -->
      </view>
    </view>
    
    <!-- 底部操作区 -->
    <view class="footer-section">
      <button class="action-btn">提交</button>
    </view>
  </view>
</template>

<style lang="scss" scoped>
.page-container {
  min-height: 100vh;
  background-color: $uni-bg-color-grey; // 使用标准变量
  padding: $uni-spacing-col-lg;
}

.card-item {
  @include hjz-card; // 使用全局 Mixin
}

.action-btn {
  @include hjz-btn-primary; // 使用全局 Mixin
}
</style>
```

**场景 B: 自定义导航栏 (工作台/大屏页)**

```html
<template>
  <view class="page-container">
    <!-- 自定义 Header -->
    <view class="custom-header">
       <!-- 包含状态栏占位、标题、背景渐变 -->
    </view>
    <!-- ... -->
  </view>
</template>
```

## 5. 检查清单 (Checklist)

在生成代码前，请自检：
1. [ ] 是否直接引用了 `uni.scss` 中的 `$uni-` 或 `$uv-` 标准变量？
2. [ ] 是否避免了在 `<style>` 中重复定义通用颜色变量？
3. [ ] 是否根据 `pages.json` 的导航栏配置决定了 Header 的写法？
4. [ ] 是否使用了 `@include hjz-card` 来保持卡片样式统一？
5. [ ] 是否在 `<style>` 中导入了 `@import '@/uni.scss';`？
6. [ ] 是否将具体业务名词抽象为通用 UI 术语？
````

#### B. 全局样式代码更新 (`uni.scss` snippet)
生成一段 SCSS 代码，用于**覆盖/更新** `uni.scss` 中的标准变量。

格式示例：
```scss
/* ========================================================================
   HJZ-AMS UI Rule (Custom Theme) - 自动更新标准变量
   ======================================================================== */

/* 1. CSS 变量覆盖 (用于主题切换) */
:root, page {
	--uv-primary: {{extracted_primary_color}};
	--uv-primary-gradient-start: {{extracted_gradient_start}};
	--uv-primary-gradient-end: {{extracted_gradient_end}};
    --uv-primary-gradient: linear-gradient(90deg, {{extracted_gradient_start}} 0%, {{extracted_gradient_end}} 100%);
    
    --uni-text-color: {{extracted_text_main}};
    --uni-text-color-sub: {{extracted_text_sub}};
    --uni-text-color-grey: {{extracted_text_placeholder}};
    
    --uni-bg-color: {{extracted_bg_card}};
    --uni-bg-color-grey: {{extracted_bg_page}};
    --uni-border-color: {{extracted_border}};
}

/* 2. SCSS 变量覆盖 (用于编译) */
// 注意：以下变量通常映射到 CSS 变量，但如果需要直接覆盖值，请在此定义

// 尺寸更新
$uni-font-size-base: {{extracted_font_base}};
$uni-border-radius-lg: {{extracted_radius_card}};
$uni-spacing-col-lg: {{extracted_spacing_page}};
$uni-spacing-card: {{extracted_spacing_card}}; // 如果标准变量里没有，可以保留 $uni- 前缀风格

// 3. 自定义补充 (仅当标准变量不足时)
$hjz-shadow-card: {{extracted_shadow}};

// 4. 混合宏
@mixin hjz-card {
  background-color: $uni-bg-color;
  border-radius: $uni-border-radius-lg;
  padding: 24rpx;
  margin-bottom: $uni-spacing-card;
  box-shadow: $hjz-shadow-card;
}

@mixin hjz-btn-primary {
  background: $uv-primary-gradient;
  color: #fff;
  border-radius: 999px;
}
```

## 2. 使用示例

用户输入：
> 使用 hjz-ui-rule-maker 分析 `ai-coder/ui-style` 目录下的设计规范，生成项目 UI Skill。

Skill 输出：
1.  读取 `ui-style` 下的文件。
2.  提取颜色如`#FF5722` (Primary), `#F5F6FA` (Bg) 等。
3.  生成新的 `hjz-ui-rule/SKILL.md` 文件 (引用 `$uni-color-primary` 等)。
4.  把生成的 SCSS 代码，直接更新到 `uni.scss` 中的 CSS 变量定义区域
