# hjz-ui-rule-getter 使用示例

本目录包含 `hjz-ui-rule-getter` Skill 的实际使用示例。

## 示例 1: 从设计稿提取 UI 规范

### 场景描述
设计师提供了小程序商城首页的设计稿，需要从中提取完整的 UI 设计规范。

### 步骤

#### 1. 准备设计稿图片

准备以下设计稿（支持本地路径或网络 URL）：
- 首页设计稿：`designs/homepage.png`
- 商品详情页：`designs/product-detail.png`
- 个人中心页：`designs/profile.png`

#### 2. 调用 hjz-ui-rule-getter Skill

**用户输入：**
```
使用 hjz-ui-rule-getter 分析 designs/homepage.png 生成 UI 设计规范
```

**AI 执行过程：**

1. 读取设计稿图片
2. 分析各个设计维度
3. 生成标准化规范文档

#### 3. 输出的设计规范文档

**生成文件：** `ui-design-spec.md`

```markdown
# UI 设计规范 - 商城项目

## 一：色彩规范

| 类别 | 项目 | 色值 | 用途 |
|------|------|------|------|
| 主色 | 主色 | `#FF6B35` | 顶部导航栏、主要按钮、价格文字 |
| 辅助色 | 辅助色1 | `#FFB800` | 促销标签、评分星级 |
| 辅助色 | 辅助色2 | `#4CAF50` | 成功状态、库存充足标识 |
| 中性色 | 背景色1 | `#F5F6FA` | 页面背景 |
| 中性色 | 背景色2 | `#FFFFFF` | 卡片背景 |
| 中性色 | 文字色1 | `#333333` | 标题文字 |
| 中性色 | 文字色2 | `#666666` | 正文内容 |
| 中性色 | 文字色3 | `#999999` | 辅助说明、占位符 |
| 功能色 | 错误色 | `#FF4D4F` | 删除按钮、错误提示 |
| 功能色 | 警告色 | `#FAAD14` | 库存紧张提示 |

**使用说明**：
- 主色用于核心交互元素（购买按钮、选中状态）
- 辅助色用于强调特殊状态（促销、优惠）
- 中性色用于构建页面层次结构

## 二：字体规范

| 类别 | 字号 | 字重 | 行高 | 用途 |
|------|------|------|------|------|
| 特大标题 | 24px | Bold | 32px | 活动标题、Banner 文字 |
| 大标题 | 20px | Bold | 28px | 页面主标题、模块标题 |
| 中标题 | 16px | Medium | 24px | 商品名称、列表项标题 |
| 正文 | 14px | Regular | 20px | 商品描述、按钮文字 |
| 小字 | 12px | Regular | 16px | 辅助说明、时间信息 |
| 价格文字 | 18px | Bold | 24px | 商品价格显示 |

**字体家族**：
- 中文字体：PingFang SC, 苹方
- 英文字体/数字：SF Pro, -apple-system

## 三：图标规范

| 类别 | 尺寸 | 风格 | 颜色 | 用途 |
|------|------|------|------|------|
| 导航图标 | 24x24px | 线性 | `#333333` | 返回、搜索、菜单 |
| 功能图标 | 48x48px | 面性 | 主色 | 核心功能入口 |
| 标签图标 | 16x16px | 面性 | 辅助色 | 促销标签、状态标识 |
| 底部导航 | 28x28px | 面性/线性切换 | 主色/灰色 | Tab Bar 图标 |

**图标风格**：
- 统一使用圆角设计
- 线性图标线条粗细：2px
- 面性图标填充饱满

## 四：视觉效果与组件样式

### 1. 渐变效果
- **场景**：顶部导航栏背景
- **参数**：
  - 起始色：`#FF6B35`
  - 结束色：`#FF8F65`
  - 方向：从上到下 180deg

### 2. 阴影效果
- **卡片阴影**：
  - X偏移：0px
  - Y偏移：2px
  - 模糊半径：8px
  - 颜色：`rgba(0, 0, 0, 0.08)`
- **按钮阴影**：
  - X偏移：0px
  - Y偏移：4px
  - 模糊半径：12px
  - 颜色：`rgba(255, 107, 53, 0.3)`

### 3. 圆角系统
- **大圆角**：16px（卡片、弹窗）
- **中圆角**：12px（搜索框、输入框）
- **小圆角**：8px（标签、小按钮）
- **胶囊圆角**：999px（主要按钮）

### 4. 间距与布局
- **页面边距**：16px
- **卡片间距**：12px
- **卡片内边距**：16px
- **元素间距**：8px
- **列表项间距**：12px

## 五：组件设计规范

### 1. 顶部导航栏
- 渐变背景（主色渐变）
- 高度：44px + 状态栏高度
- 左侧：返回图标
- 中间：页面标题
- 右侧：搜索/菜单图标
- 底部圆角：16px

### 2. 商品卡片
- 白色背景
- 圆角：16px
- 阴影：卡片阴影
- 内边距：16px
- 包含：商品图片、名称、价格、促销标签

### 3. 主要按钮
- 背景：主色渐变
- 圆角：999px（胶囊形）
- 高度：48px
- 文字：白色、16px、Medium
- 阴影：按钮阴影

### 4. 底部导航栏 (Tab Bar)
- 背景：白色
- 高度：50px + 安全区域
- 图标：28x28px
- 文字：12px
- 选中态：主色
- 未选中态：`#999999`

## 六：代码实现建议

### SCSS 变量定义
```scss
// 色彩变量
$primary-color: #FF6B35;
$primary-gradient-end: #FF8F65;
$secondary-color: #FFB800;
$success-color: #4CAF50;
$error-color: #FF4D4F;
$warning-color: #FAAD14;

$bg-page: #F5F6FA;
$bg-card: #FFFFFF;

$text-primary: #333333;
$text-secondary: #666666;
$text-placeholder: #999999;

// 字体变量
$font-family-chinese: 'PingFang SC', sans-serif;
$font-family-english: 'SF Pro', -apple-system, sans-serif;

$font-size-xxl: 24px;
$font-size-xl: 20px;
$font-size-lg: 16px;
$font-size-base: 14px;
$font-size-sm: 12px;
$font-size-price: 18px;

// 间距变量
$page-margin: 16px;
$card-margin: 12px;
$card-padding: 16px;
$element-spacing: 8px;

// 圆角变量
$radius-large: 16px;
$radius-medium: 12px;
$radius-small: 8px;
$radius-pill: 999px;

// 阴影变量
$shadow-card: 0 2px 8px rgba(0, 0, 0, 0.08);
$shadow-button: 0 4px 12px rgba(255, 107, 53, 0.3);
```

### 组件样式示例
```scss
// 商品卡片
.product-card {
  background: $bg-card;
  border-radius: $radius-large;
  padding: $card-padding;
  margin-bottom: $card-margin;
  box-shadow: $shadow-card;
  
  .product-name {
    font-size: $font-size-lg;
    font-weight: 500;
    color: $text-primary;
  }
  
  .product-price {
    font-size: $font-size-price;
    font-weight: 700;
    color: $primary-color;
  }
}

// 主要按钮
.btn-primary {
  background: linear-gradient(180deg, $primary-color 0%, $primary-gradient-end 100%);
  border-radius: $radius-pill;
  height: 48px;
  color: white;
  font-size: $font-size-lg;
  font-weight: 500;
  box-shadow: $shadow-button;
}
```
```

---

## 示例 2: 多页面综合分析

### 场景描述
需要分析多个页面以获得更完整的设计规范。

**用户输入：**
```
使用 hjz-ui-rule-getter 综合分析以下设计稿，生成完整的 UI 规范：
- designs/homepage.png
- designs/product-detail.png
- designs/cart.png
- designs/profile.png
```

**AI 执行过程：**
1. 依次读取所有设计稿
2. 对比分析各页面的设计规范
3. 提取共同的设计语言和差异化的特殊规则
4. 生成更全面的规范文档

---

## 示例 3: 现有项目规范提取

### 场景描述
对一个已上线的小项目进行设计规范提取，用于后续统一风格。

**用户输入：**
```
使用 hjz-ui-rule-getter 分析当前项目的截图，提取 UI 规范用于后续开发统一标准
```

**输出内容：**
- 现有设计规范文档
- 发现的问题和建议
- 需要统一的样式清单

---

## 注意事项

1. **图片质量**：提供高分辨率的设计稿，避免模糊图片
2. **完整性**：尽量提供包含各种组件和状态的页面
3. **抽象化**：生成的规范会将具体业务名词抽象为通用 UI 术语
4. **持续更新**：设计迭代后应及时更新规范文档
5. **多页面分析**：建议分析 3-5 个核心页面以获得完整规范
