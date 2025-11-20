[根目录](../../../CLAUDE.md) > [src](../../) > **formatters**

# formatters - 数据格式化模块

## 模块职责

formatters 模块负责将浏览器操作和监控产生的原始数据转换为结构化、用户友好的格式，支持控制台消息、网络请求和页面快照等多种数据类型的格式化处理。

## 格式化器架构

### 统一格式化接口
所有格式化器遵循统一的输出格式：
- 结构化数据展示
- 人类可读的文本描述
- 支持分页和过滤
- 标准化的元数据信息

### 输出格式标准
- **文本格式**: 清晰的层次结构和描述
- **数据格式**: JSON 结构化数据
- **分页支持**: 大数据集的分页展示
- **过滤机制**: 基于类型和条件的数据筛选

## 核心格式化器

### 1. consoleFormatter.ts
**功能**: 控制台消息格式化

**处理内容**:
- 浏览器控制台日志
- JavaScript 错误信息
- 警告和信息消息
- 用户自定义输出

**格式化特性**:
```typescript
// 示例输出格式
[Console] Info: Page loaded successfully
[Error] TypeError: Cannot read property 'x' of undefined
  at Object.handler (script.js:12:8)
```

**支持的消息类型**:
- `log`: 一般日志消息
- `info`: 信息消息
- `warn`: 警告消息
- `error`: 错误消息
- `debug`: 调试消息

### 2. networkFormatter.ts
**功能**: 网络请求数据格式化

**处理内容**:
- HTTP/HTTPS 请求详情
- 请求头和响应头
- 请求体和响应体
- 时间线和性能指标

**格式化特性**:
```typescript
// 示例输出格式
[Network] GET https://example.com/api/data
  Status: 200 OK
  Duration: 245ms
  Size: 1.2 KB
  Content-Type: application/json
```

**关键信息展示**:
- 请求方法和 URL
- 响应状态码和消息
- 请求耗时和大小
- 内容类型和编码

### 3. snapshotFormatter.ts
**功能**: 页面快照格式化

**处理内容**:
- 可访问性树结构
- DOM 元素信息
- 元素属性和状态
- 页面结构层次

**格式化特性**:
```typescript
// 示例输出格式
[Page] Document
  └─ [Heading] Welcome to my site (h1)
     └─ [Button] Submit (button)
        └─ [Text] Click me (text)
```

**结构化展示**:
- 层级树形结构
- 元素类型和角色
- 可访问性属性
- 交互状态信息

## 格式化流程

### 1. 数据收集
- 从浏览器 API 获取原始数据
- 事件监听和数据聚合
- 状态缓存和更新

### 2. 数据处理
- 数据清洗和标准化
- 重复数据去除
- 时序排序和分组

### 3. 格式化输出
- 应用格式化规则
- 生成可读文本
- 添加上下文信息

### 4. 分页和过滤
- 大数据集分页处理
- 基于条件的数据过滤
- 排序和分组选项

## 配置选项

### 通用配置
- `verbose`: 详细模式输出
- `includeMetadata`: 包含元数据信息
- `maxItems`: 最大显示项目数
- `sortBy`: 排序字段

### 格式化器特定配置
- **控制台**: 消息类型过滤、堆栈跟踪深度
- **网络**: 状态码过滤、域名过滤
- **快照**: 详细程度、元素过滤

## 性能优化

### 数据缓存
- 格式化结果缓存
- 增量更新机制
- 内存使用优化

### 懒加载
- 大数据集按需加载
- 分页数据延迟处理
- 后台格式化队列

## 测试策略

### 快照测试
- 输出格式稳定性验证
- 回归检测
- 格式变更影响评估

### 单元测试
- 各种数据类型处理
- 边界条件测试
- 性能基准测试

## 集成方式

### 与响应系统集成
```typescript
// 在工具中使用格式化器
response.setIncludeConsoleData(true, {
  types: ['error', 'warn'],
  limit: 50
});
```

### 自定义格式化
- 扩展现有格式化器
- 添加新的数据类型支持
- 自定义输出模板

## 常见问题 (FAQ)

### Q: 如何自定义输出格式？
A: 可以通过配置选项调整输出详细程度，或扩展格式化器添加自定义格式。

### Q: 大数据集如何处理？
A: 使用分页机制和过滤选项，避免一次性加载过多数据。

### Q: 如何添加新的数据类型？
A: 创建新的格式化器文件，实现统一的格式化接口。

## 相关文件清单

### 核心格式化器
- `consoleFormatter.ts` - 控制台消息格式化
- `networkFormatter.ts` - 网络请求数据格式化
- `snapshotFormatter.ts` - 页面快照格式化

### 测试文件
- `consoleFormatter.test.ts` - 控制台格式化测试
- `networkFormatter.test.ts` - 网络格式化测试
- `snapshotFormatter.test.ts` - 快照格式化测试

## 变更记录 (Changelog)

### 2025-01-20 - 格式化模块分析完成
- 识别出 3 个核心格式化器
- 分析出统一的数据处理流程
- 发现完整的分页和过滤机制
- 格式化覆盖率：100%，设计规范，扩展性强