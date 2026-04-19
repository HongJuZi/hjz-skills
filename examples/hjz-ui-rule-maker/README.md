# hjz-ui-rule-maker 使用示例

本目录包含 `hjz-ui-rule-maker` Skill 的实际使用示例。

## 示例 1: 从设计规范生成项目专属 Skill

### 场景描述
设计师提供了完整的 UI 设计规范文档和 SCSS 变量文件，需要将其转化为 AI IDE 可执行的 Skill。

### 步骤

#### 1. 准备设计资料

**目录结构：** `ai-coder/ui-style/`

```
ai-coder/ui-style/
├── ui-design-spec.md      # UI 设计规范文档
└── variables.scss         # SCSS 变量定义
```

**ui-design-spec.md 内容：**
```markdown
# 商城 UI 设计规范

## 色彩系统
- 主色：#FF6B35
- 渐变起始：#FF6B35
- 渐变结束：#FF8F65
- 页面背景：#F5F6FA
- 卡片背景：#FFFFFF

## 字体规范
- 基础字号：14px
- 大标题：20px
- 中标题：16px

## 间距系统
- 页面边距：16px (32rpx)
- 卡片间距：12px (24rpx)

## 圆角
- 卡片圆角：16px (32rpx)
```

**variables.scss 内容：**
```scss
$primary-color: #FF6B35;
$primary-gradient-end: #FF8F65;
$bg-page: #F5F6FA;
$bg-card: #FFFFFF;
$text-primary: #333333;
$text-secondary: #666666;
$border-color: #E5E5E5;
$shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
```

#### 2. 调用 hjz-ui-rule-maker Skill

**用户输入：**
```
使用 hjz-ui-rule-maker 分析 ai-coder/ui-style 目录下的设计规范，生成项目 UI Skill
```

#### 3. AI 执行过程

**步骤 1: 分析阶段**

读取设计文件，提取 Design Tokens 并映射到 Uniapp 标准变量：

| 设计 Token | 提取值 | 映射到标准变量 |
|-----------|--------|---------------|
| 主色 | `#FF6B35` | `$uni-color-primary` / `$uv-primary` |
| 渐变起始 | `#FF6B35` | `$uv-primary-gradient-start` |
| 渐变结束 | `#FF8F65` | `$uv-primary-gradient-end` |
| 页面背景 | `#F5F6FA` | `$uni-bg-color-grey` |
| 卡片背景 | `#FFFFFF` | `$uni-bg-color` |
| 卡片圆角 | `16px` (32rpx) | `$uni-border-radius-lg` |
| 页面边距 | `16px` (32rpx) | `$uni-spacing-col-lg` |

**步骤 2: 生成项目专属 Skill**

**生成文件：** `.trae/skills/hjz-ui-rule/SKILL.md`

```markdown
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
| **主色** | `$uni-color-primary` / `$uv-primary` | `#FF6B35` | 核心按钮、选中状态 |
| **渐变起始** | `$uv-primary-gradient-start` | `#FF6B35` | 顶部背景渐变起始色 |
| **渐变结束** | `$uv-primary-gradient-end` | `#FF8F65` | 顶部背景渐变结束色 |
| **页面背景** | `$uni-bg-color-grey` | `#F5F6FA` | 整体页面底色 |
| **卡片背景** | `$uni-bg-color` | `#FFFFFF` | 内容卡片背景 |
| **圆角** | `$uni-border-radius-lg` | `32rpx` | 卡片圆角 |

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
- **页面边距**: `$uni-spacing-col-lg` (32rpx)
- **卡片间距**: 24rpx

## 4. 代码生成指南 (Coding Guidelines)

### 4.1 Vue 3 + Setup 语法
- 必须使用 `<script setup>` 语法。
- 必须使用 `uni-app` 的内置组件 (`view`, `text`, `image`, `scroll-view`) 或 `uv-ui` 组件。
- **严禁** 使用 HTML 标签如 `div`, `span`, `img`。

### 4.2 SCSS 编写规范 (High Reuse)
- **引用全局变量**: 在 `<style lang="scss" scoped>` 中直接使用 `uni.scss` 中的变量。
- **使用 Mixins**: 对于卡片容器，直接使用 `@include hjz-card;`。
- **单位**: 统一使用 `rpx`。

### 4.3 导航栏处理 (Navigation Bar)
- **Default (默认)**: 如果 `pages.json` 中未设置 `navigationStyle: "custom"`，则不需要在 template 中编写顶部 Header。
- **Custom (自定义)**: 如果配置了 `navigationStyle: "custom"`，则必须在 template 顶部编写自定义 Header。

### 4.4 模板结构示例

**场景 A: 默认导航栏**

```html
<template>
  <view class="page-container">
    <view class="content-section">
      <view class="card-item" v-for="(item, index) in list" :key="index">
        <!-- 卡片内容 -->
      </view>
    </view>
  </view>
</template>

<style lang="scss" scoped>
@import '@/uni.scss';

.page-container {
  min-height: 100vh;
  background-color: $uni-bg-color-grey;
  padding: $uni-spacing-col-lg;
}

.card-item {
  @include hjz-card;
}
</style>
```

## 5. 检查清单 (Checklist)

在生成代码前，请自检：
1. [ ] 是否直接引用了 `uni.scss` 中的 `$uni-` 或 `$uv-` 标准变量？
2. [ ] 是否避免了在 `<style>` 中重复定义通用颜色变量？
3. [ ] 是否根据 `pages.json` 的导航栏配置决定了 Header 的写法？
4. [ ] 是否使用了 `@include hjz-card` 来保持卡片样式统一？
5. [ ] 是否在 `<style>` 中导入了 `@import '@/uni.scss';`？
6. [ ] 是否将具体业务名词抽象为通用 UI 术语？
```

**步骤 3: 生成 uni.scss 更新代码**

**输出 SCSS 代码片段（用于更新 uni.scss）：**

```scss
/* ========================================================================
   HJZ-Shop UI Rule (Custom Theme) - 自动更新标准变量
   ======================================================================== */

/* 1. CSS 变量覆盖 (用于主题切换) */
:root, page {
	--uv-primary: #FF6B35;
	--uv-primary-gradient-start: #FF6B35;
	--uv-primary-gradient-end: #FF8F65;
    --uv-primary-gradient: linear-gradient(90deg, #FF6B35 0%, #FF8F65 100%);
    
    --uni-text-color: #333333;
    --uni-text-color-sub: #666666;
    --uni-text-color-grey: #999999;
    
    --uni-bg-color: #FFFFFF;
    --uni-bg-color-grey: #F5F6FA;
    --uni-border-color: #E5E5E5;
}

/* 2. SCSS 变量覆盖 (用于编译) */
$uni-font-size-base: 14px;
$uni-border-radius-lg: 32rpx;
$uni-spacing-col-lg: 32rpx;

/* 3. 自定义补充 (仅当标准变量不足时) */
$hjz-shadow-card: 0 2px 8px rgba(0, 0, 0, 0.08);

/* 4. 混合宏 */
@mixin hjz-card {
  background-color: $uni-bg-color;
  border-radius: $uni-border-radius-lg;
  padding: 24rpx;
  margin-bottom: 24rpx;
  box-shadow: $hjz-shadow-card;
}

@mixin hjz-btn-primary {
  background: linear-gradient(180deg, $uni-color-primary 0%, $uv-primary-gradient-end 100%);
  color: #fff;
  border-radius: 999px;
}
```

#### 4. 应用生成的代码

将上述 SCSS 代码片段添加到项目的 `uni.scss` 文件中：

```bash
# uni.scss 文件位置通常在项目根目录或 styles 目录
your-project/uni.scss
```

---

## 示例 2: 更新现有项目主题

### 场景描述
项目需要更换主题色，从蓝色系改为橙色系。

**用户输入：**
```
使用 hjz-ui-rule-maker 分析 ai-coder/ui-style-new 目录，更新现有项目主题
```

**AI 执行过程：**
1. 读取新的设计规范
2. 提取新的 Design Tokens（主色：`#FF6B35`）
3. 更新 `.trae/skills/hjz-ui-rule/SKILL.md`
4. 生成新的 `uni.scss` 更新代码
5. 自动更新 `uni.scss` 中的 CSS 变量定义区域

---

## 示例 3: 多主题支持

### 场景描述
项目需要支持多主题切换（日间模式/夜间模式）。

**用户输入：**
```
使用 hjz-ui-rule-maker 分析 ai-coder/themes 目录，生成支持多主题的 UI Skill
```

**AI 额外输出：**
```scss
/* 多主题支持 */
:root, page {
  /* 日间模式 */
  --uni-bg-color: #FFFFFF;
  --uni-text-color: #333333;
}

[data-theme="dark"], page[theme="dark"] {
  /* 夜间模式 */
  --uni-bg-color: #1A1A1A;
  --uni-text-color: #E5E5E5;
  --uni-bg-color-grey: #2D2D2D;
}
```

---

## 设计 Token 映射规则

| 设计元素 | 标准变量映射 | 说明 |
|---------|------------|------|
| 主色 | `$uni-color-primary` / `$uv-primary` | 核心交互色 |
| 成功色 | `$uni-color-success` / `$uv-success` | 成功状态 |
| 警告色 | `$uni-color-warning` / `$uv-warning` | 警告状态 |
| 错误色 | `$uni-color-error` / `$uv-error` | 错误状态 |
| 页面背景 | `$uni-bg-color-grey` | 整体页面底色 |
| 卡片背景 | `$uni-bg-color` | 内容区背景 |
| 基础字号 | `$uni-font-size-base` | 正文字号 |
| 大字号 | `$uni-font-size-lg` | 标题字号 |
| 卡片圆角 | `$uni-border-radius-lg` | 统一圆角 |
| 页面边距 | `$uni-spacing-col-lg` | 标准间距 |

---

## 注意事项

1. **优先复用**：优先使用 Uniapp 和 uv-ui 的标准变量，减少自定义变量
2. **变量映射**：确保设计 Token 正确映射到标准变量
3. **CSS 变量**：使用 CSS 变量支持运行时主题切换
4. **Mixins**：合理使用 Mixins 提高代码复用性
5. **单位统一**：Uniapp 项目中统一使用 `rpx` 单位
6. **Skill 更新**：设计规范变更后及时更新 Skill 文件
7. **团队协作**：设计和开发团队共同维护规范文档
