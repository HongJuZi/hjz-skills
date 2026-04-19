# HJZ Skills

[中文版](./README_ZH.md)

## Project Introduction

This is an open-source project sharing a series of AI Skills we've accumulated during our actual development process. These Skills are skill files based on AI programming assistants like Trae IDE, designed to help development teams improve efficiency, unify code standards, and accelerate project development workflows.

Through this open-source project, we hope to exchange and share experiences with developers in AI-assisted development, exploring best practices of AI in software development together.

## Skills List

### 1. hjz-exe-sql - SQL Generation & Execution Tool

**Features:**
- Automatically generates CREATE TABLE SQL statements from POPO (Plain Old PHP Object) definitions
- Reads project database configuration and executes SQL files
- Supports automatic field type mapping (id, uniacid, create_time, status, etc.)

**Use Cases:**
- Creating database tables for new modules
- SQL synchronization after POPO definition changes
- Executing manual SQL files

### 2. hjz-ui-rule-getter - UI Design Rule Extraction Tool

**Features:**
- Automatically analyzes and extracts complete design rules from UI design mockup images
- Includes color systems, typography rules, icon specifications, layout rules, component styles, etc.
- Generates standardized UI design rule documents (Markdown format)
- Provides SCSS variable definitions and component style examples

**Use Cases:**
- Quickly generating UI rules from design mockups when starting new projects
- Design refactoring and rule unification for existing projects
- Design review and consistency checking

### 3. hjz-ui-rule-maker - UI Rule to Executable Skill Converter

**Features:**
- Converts static UI design rule documents into executable standard Skills for AI IDEs
- Automatically maps Design Tokens to Uniapp/uv-ui standard variables
- Generates project-specific `hjz-ui-rule` Skill files
- Automatically updates global style configuration (uni.scss)

**Use Cases:**
- Converting designer-provided design rules into executable AI Skills for development teams
- Automatic project theme adaptation
- Unifying team UI development standards

## Quick Start

### Prerequisites

- Trae IDE or other AI programming assistants that support Skills
- Node.js environment (for frontend project development)
- PHP environment (for backend project development, hjz-exe-sql)
- MySQL database (hjz-exe-sql)

### Installation & Usage

1. Clone this repository:
```bash
git clone https://github.com/your-org/hjz-skills.git
```

2. Copy the needed Skill files to your project:
```bash
# Copy to your project's .trae/skills/ directory
cp -r hjz-skills/hjz-exe-sql your-project/.trae/skills/
cp -r hjz-skills/hjz-ui-rule-getter your-project/.trae/skills/
cp -r hjz-skills/hjz-ui-rule-maker your-project/.trae/skills/
```

3. Call the corresponding Skill in your AI IDE to use it

## Usage Examples

### hjz-exe-sql Examples

#### Example 1: Generate and Execute SQL from POPO File

**Scenario:** Create a new product module that requires generating a database table from POPO definition.

**Steps:**

1. **Prepare POPO File** - `ProductPopo.php`

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

2. **Call hjz-exe-sql Skill**

User input:
```
Use hjz-exe-sql to generate and execute SQL for ProductPopo.php
```

3. **AI Generates SQL**

```sql
CREATE TABLE `h_product` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT PRIMARY KEY,
  `uniacid` int(11) unsigned NOT NULL DEFAULT '0',
  `product_name` varchar(255) NOT NULL COMMENT 'Product Name',
  `product_code` varchar(255) NOT NULL COMMENT 'Product Code',
  `price` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT 'Price',
  `stock` int(11) NOT NULL DEFAULT '0' COMMENT 'Stock',
  `category_id` int(11) DEFAULT NULL COMMENT 'Category ID',
  `status` tinyint(1) NOT NULL DEFAULT '1' COMMENT 'Status',
  `is_hot` tinyint(1) NOT NULL DEFAULT '0' COMMENT 'Is Hot',
  `description` text COMMENT 'Description',
  `create_time` datetime DEFAULT CURRENT_TIMESTAMP COMMENT 'Create Time',
  `update_time` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'Update Time'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='Product Table';
```

4. **Confirm and Execute**

AI reads database configuration and asks for confirmation:
```
⚠️ Ready to execute SQL file:
- Database: h_shop_db
- Host: 127.0.0.1:3306
- SQL File: /backend/ai-coder/ai-ok/create_product_table_20260419.sql

Confirm execution? (yes/no)
```

5. **Execution Result**
```
✅ SQL executed successfully! Table `h_product` created.
```

**Field Mapping Rules:**

| POPO Field | Database Type | Description |
|----------|-----------|------|
| `id` | `int(11) unsigned AUTO_INCREMENT PRIMARY KEY` | Primary Key |
| `uniacid` | `int(11) unsigned NOT NULL DEFAULT '0'` | Mini-program/Official Account ID |
| `create_time` | `datetime` or `TIMESTAMP` | Create Time |
| `status` / `is_*` | `tinyint(1)` | Status/Boolean Field |
| `*_id` suffix | `int(11)` | Foreign Key ID |
| `verify => numeric` | `decimal` or `int` | Numeric Field |
| Default | `varchar(255)` | String Field |

For more examples, see: [examples/hjz-exe-sql/README.md](./examples/hjz-exe-sql/README.md)

---

### hjz-ui-rule-getter Examples

#### Example 1: Extract UI Rules from Design Mockup

**Scenario:** Designer provided a mini-program mall homepage design mockup, need to extract complete UI design rules.

**Steps:**

1. **Prepare Design Mockup Image**

Prepare the design mockup file (supports local path or network URL)

2. **Call hjz-ui-rule-getter Skill**

User input:
```
Use hjz-ui-rule-getter to analyze designs/homepage.png and generate UI design rules
```

3. **AI Generated Design Rule Document**

```markdown
# UI Design Rules - Mall Project

## 1. Color Rules

| Category | Item | Value | Usage |
|------|------|------|------|
| Primary | Primary Color | `#FF6B35` | Top navbar, main buttons, price text |
| Secondary | Secondary Color 1 | `#FFB800` | Promotion tags, rating stars |
| Neutral | Background 1 | `#F5F6FA` | Page background |
| Neutral | Background 2 | `#FFFFFF` | Card background |
| Neutral | Text Color 1 | `#333333` | Title text |
| Neutral | Text Color 2 | `#666666` | Body content |

## 2. Typography Rules

| Category | Size | Weight | Usage |
|------|------|------|------|
| Large Title | 20px | Bold | Page main title, module title |
| Medium Title | 16px | Medium | Product name, list item title |
| Body | 14px | Regular | Product description, button text |
| Small Text | 12px | Regular | Auxiliary description, time info |

**Font Family**:
- Chinese: PingFang SC
- English/Numbers: SF Pro, -apple-system

## 3. Visual Effects

### Corner Radius System
- **Large Radius**: 16px (cards, dialogs)
- **Medium Radius**: 12px (search box, input box)
- **Small Radius**: 8px (tags, small buttons)
- **Pill Radius**: 999px (main buttons)

### Shadow Effects
- **Card Shadow**: 0 2px 8px rgba(0, 0, 0, 0.08)
- **Button Shadow**: 0 4px 12px rgba(255, 107, 53, 0.3)

## 4. SCSS Variable Definitions

```scss
// Color variables
$primary-color: #FF6B35;
$bg-page: #F5F6FA;
$bg-card: #FFFFFF;
$text-primary: #333333;

// Font variables
$font-size-xl: 20px;
$font-size-lg: 16px;
$font-size-base: 14px;
$font-size-sm: 12px;

// Spacing variables
$page-margin: 16px;
$card-margin: 12px;

// Radius variables
$radius-large: 16px;
$radius-pill: 999px;
```

## 5. Component Style Examples

```scss
// Product Card
.product-card {
  background: $bg-card;
  border-radius: $radius-large;
  padding: 16px;
  margin-bottom: $card-margin;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

// Primary Button
.btn-primary {
  background: $primary-color;
  border-radius: $radius-pill;
  height: 48px;
  color: white;
}
```
```

**Notes:**
- Provide high-resolution design mockups, avoid blurry images
- Try to provide pages containing various components and states
- Generated rules will abstract specific business terms into general UI terminology
- Recommend analyzing 3-5 core pages to obtain complete rules

For more examples, see: [examples/hjz-ui-rule-getter/README.md](./examples/hjz-ui-rule-getter/README.md)

---

### hjz-ui-rule-maker Examples

#### Example 1: Generate Project-Specific Skill from Design Rules

**Scenario:** Designer provided complete UI design rule documents and SCSS variable files, need to convert them into AI IDE executable Skills.

**Steps:**

1. **Prepare Design Materials**

```
ai-coder/ui-style/
├── ui-design-spec.md      # UI Design Rule Document
└── variables.scss         # SCSS Variable Definitions
```

2. **Call hjz-ui-rule-maker Skill**

User input:
```
Use hjz-ui-rule-maker to analyze design rules in ai-coder/ui-style directory and generate project UI Skill
```

3. **AI Analyzes and Maps Design Tokens**

| Design Token | Extracted Value | Mapped to Standard Variable |
|-----------|--------|---------------|
| Primary Color | `#FF6B35` | `$uni-color-primary` / `$uv-primary` |
| Gradient Start | `#FF6B35` | `$uv-primary-gradient-start` |
| Gradient End | `#FF8F65` | `$uv-primary-gradient-end` |
| Page Background | `#F5F6FA` | `$uni-bg-color-grey` |
| Card Background | `#FFFFFF` | `$uni-bg-color` |
| Card Radius | `16px` (32rpx) | `$uni-border-radius-lg` |

4. **Generate Project-Specific Skill**

**Generated File:** `.trae/skills/hjz-ui-rule/SKILL.md`

This Skill includes:
- Core design principles
- Global style configuration
- Design rule details
- Code generation guidelines
- Template structure examples
- Checklist

5. **Generate uni.scss Update Code**

```scss
/* HJZ-Shop UI Rule (Custom Theme) */

/* 1. CSS Variable Override */
:root, page {
	--uv-primary: #FF6B35;
	--uv-primary-gradient-start: #FF6B35;
	--uv-primary-gradient-end: #FF8F65;
    --uni-bg-color: #FFFFFF;
    --uni-bg-color-grey: #F5F6FA;
    --uni-text-color: #333333;
}

/* 2. SCSS Variable Override */
$uni-font-size-base: 14px;
$uni-border-radius-lg: 32rpx;
$uni-spacing-col-lg: 32rpx;

/* 3. Mixins */
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

6. **Apply Generated Code**

Add the above SCSS code to your project's `uni.scss` file.

**Notes:**
- Prioritize using Uniapp and uv-ui standard variables, reduce custom variables
- Ensure design tokens are correctly mapped to standard variables
- Use CSS variables to support runtime theme switching
- Uniformly use `rpx` units
- Update Skill files promptly when design rules change

For more examples, see: [examples/hjz-ui-rule-maker/README.md](./examples/hjz-ui-rule-maker/README.md)

---

## Project Structure

```
hjz-skills/
├── hjz-exe-sql/              # SQL Generation & Execution Skill
│   └── SKILL.md
├── hjz-ui-rule-getter/       # UI Rule Extraction Skill
│   ├── README.md
│   └── SKILL.md
├── hjz-ui-rule-maker/        # UI Rule Conversion Skill
│   └── SKILL.md
├── examples/                 # Usage Examples Directory
│   ├── hjz-exe-sql/
│   ├── hjz-ui-rule-getter/
│   └── hjz-ui-rule-maker/
├── README.md                 # Project Documentation (English - Default)
├── README_ZH.md              # 项目说明文档（中文）
└── LICENSE
```

## Contributing

We welcome all forms of contributions, including but not limited to:

- Submitting bug reports and feature requests
- Submitting Pull Requests to improve existing Skills
- Sharing your experience using these Skills
- Submitting new Skills

### Contribution Steps

1. Fork this repository
2. Create your feature branch (`git checkout -b feature/amazing-skill`)
3. Commit your changes (`git commit -m 'Add some amazing skill'`)
4. Push to the branch (`git push origin feature/amazing-skill`)
5. Submit a Pull Request

## License

This project is open-sourced under the [MIT License](LICENSE).

## Contact Us

If you have any questions or suggestions, feel free to contact us through:

- Submit a GitHub Issue
- Send an email to: your-email@example.com
