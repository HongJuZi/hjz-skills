# HJZ Skills

[English Version](./README.md)

## 项目简介

这是一个开源项目，分享了我们在公司实际开发过程中积累的一系列 AI Skills。这些 Skills 是基于 Trae IDE 等 AI 编程助手的技能文件，旨在帮助开发团队提高开发效率、统一代码规范、加速项目开发流程。

我们希望通过这个开源项目，与各位开发者进行 AI 辅助开发方面的交流与分享，共同探索 AI 在软件开发中的最佳实践。

## Skills 列表

### 1. hjz-exe-sql - SQL 生成与执行工具

**功能简介：**
- 根据 POPO（Plain Old PHP Object）定义自动生成 CREATE TABLE SQL 语句
- 读取项目数据库配置并执行 SQL 文件
- 支持字段类型自动映射（id、uniacid、create_time、status 等）

**适用场景：**
- 新模块数据库表创建
- POPO 定义变更后的 SQL 同步
- 手动 SQL 文件的执行

### 2. hjz-ui-rule-getter - UI 设计规范提取工具

**功能简介：**
- 从 UI 设计稿图片中自动分析并提取完整的设计规范
- 包含色彩系统、字体规范、图标规范、布局规范、组件样式等
- 生成标准化的 UI 设计规范文档（Markdown 格式）
- 提供 SCSS 变量定义和组件样式示例

**适用场景：**
- 新项目启动时从设计稿快速生成 UI 规范
- 现有项目设计重构与规范统一
- 设计评审与一致性检查

### 3. hjz-ui-rule-maker - UI 规范转化为可执行 Skill

**功能简介：**
- 将静态 UI 设计规范文档转化为 AI IDE 可执行的标准 Skill
- 自动映射 Design Tokens 到 Uniapp/uv-ui 标准变量
- 生成项目专属的 `hjz-ui-rule` Skill 文件
- 自动更新全局样式配置（uni.scss）

**适用场景：**
- 将设计师提供的设计规范转化为开发团队可执行的 AI Skill
- 项目主题自动适配
- 统一团队 UI 开发规范

## 快速开始

### 前置要求

- Trae IDE 或其他支持 Skills 的 AI 编程助手
- Node.js 环境（用于前端项目开发）
- PHP 环境（用于后端项目开发，hjz-exe-sql）
- MySQL 数据库（hjz-exe-sql）

### 安装使用

1. 克隆本仓库：
```bash
git clone https://github.com/your-org/hjz-skills.git
```

2. 将需要的 Skill 文件复制到你的项目中：
```bash
# 复制到项目的 .trae/skills/ 目录
cp -r hjz-skills/hjz-exe-sql your-project/.trae/skills/
cp -r hjz-skills/hjz-ui-rule-getter your-project/.trae/skills/
cp -r hjz-skills/hjz-ui-rule-maker your-project/.trae/skills/
```

3. 在 AI IDE 中调用相应的 Skill 即可使用

## 使用示例

### hjz-exe-sql 示例

#### 示例 1: 从 POPO 文件生成并执行 SQL

**场景描述：** 创建一个新的商品模块，需要从 POPO 定义生成数据库表。

**步骤：**

1. **准备 POPO 文件** - `ProductPopo.php`

```php
<?php
return [
    '_table' => '#_product',
    '_fields' => [
        'id' => '',
        'uniacid' => '',
        'product_name' => ['verify' => 'required'],
        'product_code' => ['verify' => 'required'],
        'price' => ['verify' => 'numeric'],
        'stock' => ['verify' => 'numeric'],
        'category_id' => '',
        'status' => '',
        'is_hot' => '',
        'description' => '',
        'create_time' => '',
        'update_time' => '',
    ]
];
```

2. **调用 hjz-exe-sql Skill**

用户输入：
```
使用 hjz-exe-sql 为 ProductPopo.php 生成 SQL 并执行
```

3. **AI 生成 SQL**

```sql
CREATE TABLE `h_product` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT PRIMARY KEY,
  `uniacid` int(11) unsigned NOT NULL DEFAULT '0',
  `product_name` varchar(255) NOT NULL COMMENT '商品名称',
  `product_code` varchar(255) NOT NULL COMMENT '商品编码',
  `price` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '价格',
  `stock` int(11) NOT NULL DEFAULT '0' COMMENT '库存',
  `category_id` int(11) DEFAULT NULL COMMENT '分类ID',
  `status` tinyint(1) NOT NULL DEFAULT '1' COMMENT '状态',
  `is_hot` tinyint(1) NOT NULL DEFAULT '0' COMMENT '是否热门',
  `description` text COMMENT '商品描述',
  `create_time` datetime DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `update_time` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='商品表';
```

4. **确认并执行**

AI 会读取数据库配置并向用户确认：
```
⚠️ 准备执行 SQL 文件：
- 数据库：h_shop_db
- 主机：127.0.0.1:3306
- SQL 文件：/backend/ai-coder/ai-ok/create_product_table_20260419.sql

是否确认执行？(yes/no)
```

5. **执行结果**
```
✅ SQL 执行成功！表 `h_product` 已创建。
```

**字段映射规则：**

| POPO 字段 | 数据库类型 | 说明 |
|----------|-----------|------|
| `id` | `int(11) unsigned AUTO_INCREMENT PRIMARY KEY` | 主键 |
| `uniacid` | `int(11) unsigned NOT NULL DEFAULT '0'` | 小程序/公众号ID |
| `create_time` | `datetime` 或 `TIMESTAMP` | 创建时间 |
| `status` / `is_*` | `tinyint(1)` | 状态/布尔字段 |
| `*_id` 后缀 | `int(11)` | 外键ID |
| `verify => numeric` | `decimal` 或 `int` | 数字字段 |
| 默认 | `varchar(255)` | 字符串字段 |

更多示例请查看：[examples/hjz-exe-sql/README.md](./examples/hjz-exe-sql/README.md)

---

### hjz-ui-rule-getter 示例

#### 示例 1: 从设计稿提取 UI 规范

**场景描述：** 设计师提供了小程序商城首页的设计稿，需要从中提取完整的 UI 设计规范。

**步骤：**

1. **准备设计稿图片**

准备好设计稿文件（支持本地路径或网络 URL）

2. **调用 hjz-ui-rule-getter Skill**

用户输入：
```
使用 hjz-ui-rule-getter 分析 designs/homepage.png 生成 UI 设计规范
```

3. **AI 生成的设计规范文档**

```markdown
# UI 设计规范 - 商城项目

## 一：色彩规范

| 类别 | 项目 | 色值 | 用途 |
|------|------|------|------|
| 主色 | 主色 | `#FF6B35` | 顶部导航栏、主要按钮、价格文字 |
| 辅助色 | 辅助色1 | `#FFB800` | 促销标签、评分星级 |
| 中性色 | 背景色1 | `#F5F6FA` | 页面背景 |
| 中性色 | 背景色2 | `#FFFFFF` | 卡片背景 |
| 中性色 | 文字色1 | `#333333` | 标题文字 |
| 中性色 | 文字色2 | `#666666` | 正文内容 |

## 二：字体规范

| 类别 | 字号 | 字重 | 用途 |
|------|------|------|------|
| 大标题 | 20px | Bold | 页面主标题、模块标题 |
| 中标题 | 16px | Medium | 商品名称、列表项标题 |
| 正文 | 14px | Regular | 商品描述、按钮文字 |
| 小字 | 12px | Regular | 辅助说明、时间信息 |

**字体家族**：
- 中文字体：PingFang SC, 苹方
- 英文字体/数字：SF Pro, -apple-system

## 三：视觉效果

### 圆角系统
- **大圆角**：16px（卡片、弹窗）
- **中圆角**：12px（搜索框、输入框）
- **小圆角**：8px（标签、小按钮）
- **胶囊圆角**：999px（主要按钮）

### 阴影效果
- **卡片阴影**：0 2px 8px rgba(0, 0, 0, 0.08)
- **按钮阴影**：0 4px 12px rgba(255, 107, 53, 0.3)

## 四：SCSS 变量定义

```scss
// 色彩变量
$primary-color: #FF6B35;
$bg-page: #F5F6FA;
$bg-card: #FFFFFF;
$text-primary: #333333;

// 字体变量
$font-size-xl: 20px;
$font-size-lg: 16px;
$font-size-base: 14px;
$font-size-sm: 12px;

// 间距变量
$page-margin: 16px;
$card-margin: 12px;

// 圆角变量
$radius-large: 16px;
$radius-pill: 999px;
```

## 五：组件样式示例

```scss
// 商品卡片
.product-card {
  background: $bg-card;
  border-radius: $radius-large;
  padding: 16px;
  margin-bottom: $card-margin;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

// 主要按钮
.btn-primary {
  background: $primary-color;
  border-radius: $radius-pill;
  height: 48px;
  color: white;
}
```
```

**注意事项：**
- 提供高分辨率的设计稿，避免模糊图片
- 尽量提供包含各种组件和状态的页面
- 生成的规范会将具体业务名词抽象为通用 UI 术语
- 建议分析 3-5 个核心页面以获得完整规范

更多示例请查看：[examples/hjz-ui-rule-getter/README.md](./examples/hjz-ui-rule-getter/README.md)

---

### hjz-ui-rule-maker 示例

#### 示例 1: 从设计规范生成项目专属 Skill

**场景描述：** 设计师提供了完整的 UI 设计规范文档和 SCSS 变量文件，需要将其转化为 AI IDE 可执行的 Skill。

**步骤：**

1. **准备设计资料**

```
ai-coder/ui-style/
├── ui-design-spec.md      # UI 设计规范文档
└── variables.scss         # SCSS 变量定义
```

2. **调用 hjz-ui-rule-maker Skill**

用户输入：
```
使用 hjz-ui-rule-maker 分析 ai-coder/ui-style 目录下的设计规范，生成项目 UI Skill
```

3. **AI 分析并映射 Design Tokens**

| 设计 Token | 提取值 | 映射到标准变量 |
|-----------|--------|---------------|
| 主色 | `#FF6B35` | `$uni-color-primary` / `$uv-primary` |
| 渐变起始 | `#FF6B35` | `$uv-primary-gradient-start` |
| 渐变结束 | `#FF8F65` | `$uv-primary-gradient-end` |
| 页面背景 | `#F5F6FA` | `$uni-bg-color-grey` |
| 卡片背景 | `#FFFFFF` | `$uni-bg-color` |
| 卡片圆角 | `16px` (32rpx) | `$uni-border-radius-lg` |

4. **生成项目专属 Skill**

**生成文件：** `.trae/skills/hjz-ui-rule/SKILL.md`

该 Skill 包含：
- 核心设计原则
- 全局样式配置
- 设计规范详情
- 代码生成指南
- 模板结构示例
- 检查清单

5. **生成 uni.scss 更新代码**

```scss
/* HJZ-Shop UI Rule (Custom Theme) */

/* 1. CSS 变量覆盖 */
:root, page {
	--uv-primary: #FF6B35;
	--uv-primary-gradient-start: #FF6B35;
	--uv-primary-gradient-end: #FF8F65;
    --uni-bg-color: #FFFFFF;
    --uni-bg-color-grey: #F5F6FA;
    --uni-text-color: #333333;
}

/* 2. SCSS 变量覆盖 */
$uni-font-size-base: 14px;
$uni-border-radius-lg: 32rpx;
$uni-spacing-col-lg: 32rpx;

/* 3. 混合宏 */
@mixin hjz-card {
  background-color: $uni-bg-color;
  border-radius: $uni-border-radius-lg;
  padding: 24rpx;
  margin-bottom: 24rpx;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

@mixin hjz-btn-primary {
  background: linear-gradient(180deg, $uni-color-primary 0%, $uv-primary-gradient-end 100%);
  color: #fff;
  border-radius: 999px;
}
```

6. **应用生成的代码**

将上述 SCSS 代码添加到项目的 `uni.scss` 文件中。

**注意事项：**
- 优先使用 Uniapp 和 uv-ui 的标准变量，减少自定义变量
- 确保设计 Token 正确映射到标准变量
- 使用 CSS 变量支持运行时主题切换
- 统一使用 `rpx` 单位
- 设计规范变更后及时更新 Skill 文件

更多示例请查看：[examples/hjz-ui-rule-maker/README.md](./examples/hjz-ui-rule-maker/README.md)

---

## 项目结构

```
hjz-skills/
├── hjz-exe-sql/              # SQL 生成与执行 Skill
│   └── SKILL.md
├── hjz-ui-rule-getter/       # UI 规范提取 Skill
│   ├── README.md
│   └── SKILL.md
├── hjz-ui-rule-maker/        # UI 规范转化 Skill
│   └── SKILL.md
├── examples/                 # 使用示例目录
│   ├── hjz-exe-sql/
│   ├── hjz-ui-rule-getter/
│   └── hjz-ui-rule-maker/
├── README.md                 # Project Documentation (English - Default)
├── README_ZH.md              # 项目说明文档（中文）
└── LICENSE
```

## 贡献指南

我们欢迎所有形式的贡献，包括但不限于：

- 提交 Bug 报告和功能建议
- 提交 Pull Request 改进现有 Skills
- 分享你基于这些 Skills 的使用经验
- 提交新的 Skills

### 贡献步骤

1. Fork 本仓库
2. 创建你的特性分支 (`git checkout -b feature/amazing-skill`)
3. 提交你的更改 (`git commit -m 'Add some amazing skill'`)
4. 推送到分支 (`git push origin feature/amazing-skill`)
5. 提交 Pull Request

## 许可证

本项目采用 [MIT License](LICENSE) 开源协议。

## 联系我们

如果你有任何问题或建议，欢迎通过以下方式联系我们：

- 提交 GitHub Issue
- 发送邮件至：your-email@example.com
