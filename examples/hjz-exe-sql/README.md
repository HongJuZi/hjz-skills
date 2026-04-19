# hjz-exe-sql 使用示例

本目录包含 `hjz-exe-sql` Skill 的实际使用示例。

## 示例 1: 从 POPO 文件生成并执行 SQL

### 场景描述
创建一个新的商品模块，需要从 POPO 定义生成数据库表。

### 步骤

#### 1. 准备 POPO 文件

文件路径：`/backend/config/popo/ProductPopo.php`

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

#### 2. 调用 hjz-exe-sql Skill

**用户输入：**
```
使用 hjz-exe-sql 为 ProductPopo.php 生成 SQL 并执行
```

**AI 执行过程：**

1. 读取 POPO 文件
2. 根据字段映射规则生成 SQL：

```sql
-- 生成文件：/backend/ai-coder/ai-ok/create_product_table_20260419.sql

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

3. 读取数据库配置文件 `/backend/config/hconfig.php`：

```php
<?php
return [
    'dbHost' => '127.0.0.1',
    'dbUserName' => 'root',
    'dbUserPassword' => 'your_password',
    'dbPort' => '3306',
    'dbName' => 'h_shop_db',
];
```

4. 向用户确认执行：

```
⚠️ 准备执行 SQL 文件：
- 数据库：h_shop_db
- 主机：127.0.0.1:3306
- SQL 文件：/backend/ai-coder/ai-ok/create_product_table_20260419.sql

是否确认执行？(yes/no)
```

5. 用户确认后执行命令：

```bash
mysql -h 127.0.0.1 -u root -pyour_password -P 3306 h_shop_db < /backend/ai-coder/ai-ok/create_product_table_20260419.sql
```

6. 验证执行结果并反馈：

```
✅ SQL 执行成功！
表 `h_product` 已创建。
```

---

## 示例 2: 执行手动编写的 SQL 文件

### 场景描述
已经手动编写了一个更新表结构的 SQL 文件，需要执行。

### 步骤

**用户输入：**
```
使用 hjz-exe-sql 执行 ai-coder/updates/add_product_weight.sql
```

**SQL 文件内容：** `add_product_weight.sql`

```sql
ALTER TABLE `h_product` 
ADD COLUMN `weight` decimal(8,2) DEFAULT '0.00' COMMENT '商品重量(kg)' AFTER `price`,
ADD COLUMN `unit` varchar(50) DEFAULT '个' COMMENT '计量单位' AFTER `weight`;
```

**AI 执行过程：**

1. 读取数据库配置
2. 向用户确认：

```
⚠️ 准备执行 SQL 文件：
- 数据库：h_shop_db
- 主机：127.0.0.1:3306
- SQL 文件：/backend/ai-coder/updates/add_product_weight.sql

是否确认执行？(yes/no)
```

3. 执行并反馈结果

---

## 字段映射规则参考

| POPO 字段 | 数据库类型 | 说明 |
|----------|-----------|------|
| `id` | `int(11) unsigned AUTO_INCREMENT PRIMARY KEY` | 主键 |
| `uniacid` | `int(11) unsigned NOT NULL DEFAULT '0'` | 小程序/公众号ID |
| `create_time` | `datetime` 或 `TIMESTAMP` | 创建时间 |
| `update_time` | `datetime` 或 `TIMESTAMP` | 更新时间 |
| `status` / `is_*` | `tinyint(1)` | 状态/布尔字段 |
| `*_id` 后缀 | `int(11)` | 外键ID |
| `verify => numeric` | `decimal` 或 `int` | 数字字段 |
| 默认 | `varchar(255)` | 字符串字段 |

---

## 注意事项

1. **安全性**：执行 SQL 前必须向用户确认，显示目标数据库和文件路径
2. **配置要求**：确保 `hconfig.php` 中存在正确的数据库配置
3. **错误处理**：检查 MySQL 命令退出码，失败时提供错误信息
4. **备份建议**：执行 ALTER 等修改操作前建议先备份数据库
