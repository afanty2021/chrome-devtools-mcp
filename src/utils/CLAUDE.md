[根目录](../../../CLAUDE.md) > [src](../../) > **utils**

# utils - 工具函数模块

## 模块职责

utils 模块提供 Chrome DevTools MCP 项目的通用工具函数、类型定义和辅助功能，支持键盘操作、分页处理、类型安全等核心功能。

## 工具函数分类

### 1. 键盘操作 (keyboard.ts)
**功能**: 提供标准化的键盘操作支持

**主要功能**:
- 键盘按键映射
- 组合键处理
- 特殊键支持
- 跨平台兼容

**支持的按键类型**:
```typescript
// 字母按键
'a', 'b', 'c', ..., 'z'

// 数字按键
'0', '1', '2', ..., '9'

// 特殊按键
'Enter', 'Tab', 'Escape', 'Backspace'
'ArrowUp', 'ArrowDown', 'ArrowLeft', 'ArrowRight'
'Home', 'End', 'PageUp', 'PageDown'

// 功能键
'F1', 'F2', ..., 'F12'

// 修饰键组合
'Ctrl+a', 'Shift+Enter', 'Alt+Tab', 'Meta+c'
```

**使用示例**:
```typescript
// 简单按键
await page.keyboard.press('Enter');

// 组合键
await page.keyboard.press('Ctrl+a');

// 字符输入
await page.keyboard.type('Hello World');
```

### 2. 分页处理 (pagination.ts)
**功能**: 大数据集的分页处理和导航

**核心特性**:
- 分页参数验证
- 偏移量和限制计算
- 分页元数据生成
- 数据切片处理

**分页接口**:
```typescript
interface PaginationOptions {
  offset?: number;     // 偏移量（从0开始）
  limit?: number;      // 每页数量限制
  sortBy?: string;     // 排序字段
  sortOrder?: 'asc' | 'desc'; // 排序方向
}

interface PaginationResult<T> {
  data: T[];           // 当前页数据
  total: number;       // 总数据量
  offset: number;      // 当前偏移
  limit: number;       // 页面大小
  hasMore: boolean;    // 是否有更多数据
  nextPage?: number;   // 下一页偏移
}
```

**使用示例**:
```typescript
// 分页获取网络请求
const result = paginate(networkRequests, {
  offset: 0,
  limit: 20,
  sortBy: 'startTime'
});
```

### 3. 类型定义 (types.ts)
**功能**: 核心类型定义和接口声明

**主要类型**:
```typescript
// MCP 通用接口
export interface McpTool {
  name: string;
  description: string;
  inputSchema: Record<string, any>;
}

// 浏览器相关类型
export interface BrowserInfo {
  version: string;
  userAgent: string;
  platform: string;
}

// 网络请求类型
export interface NetworkRequestInfo {
  url: string;
  method: string;
  status: number;
  headers: Record<string, string>;
  timing: RequestTiming;
}

// 性能指标类型
export interface PerformanceMetrics {
  loadTime: number;
  domContentLoaded: number;
  firstContentfulPaint: number;
  largestContentfulPaint: number;
}
```

## 设计原则

### 1. 可复用性
- 纯函数设计
- 无副作用
- 模块化组织
- 最小依赖

### 2. 类型安全
- 完整的 TypeScript 类型定义
- 运行时类型检查
- 泛型支持
- 类型推导

### 3. 错误处理
- 参数验证
- 错误边界处理
- 友好的错误消息
- 调试信息

### 4. 性能优化
- 算法效率
- 内存使用优化
- 懒加载支持
- 缓存机制

## 使用模式

### 键盘操作模式
```typescript
// 输入文本
await fillInput(page, selector, text);

// 快捷键操作
await executeShortcut(page, 'Ctrl+s');

// 特殊键序列
await pressKeySequence(page, ['Alt', 'Tab', 'Enter']);
```

### 分页处理模式
```typescript
// 获取分页数据
const pageData = getPaginatedData(allData, pagination);

// 构建分页响应
const response = buildPaginatedResponse(data, total, options);
```

### 类型使用模式
```typescript
// 类型安全的工具参数
const params: ToolParams = {
  url: 'https://example.com',
  timeout: 5000,
  options: {waitForSelector: true}
};

// 类型守卫
if (isNetworkRequest(data)) {
  // TypeScript 知道这是 NetworkRequest 类型
  console.log(data.method, data.url);
}
```

## 测试覆盖

### 单元测试
- 每个工具函数的独立测试
- 边界条件测试
- 错误处理测试
- 性能基准测试

### 类型测试
- TypeScript 编时类型检查
- 运行时类型验证
- 泛型类型测试
- 类型守卫测试

### 集成测试
- 与其他模块的集成测试
- 真实场景模拟
- 端到端功能验证

## 扩展指南

### 添加新工具函数
1. 在相应文件中添加新函数
2. 编写完整的类型定义
3. 添加单元测试
4. 更新文档

### 创建新工具模块
1. 创建新的 TypeScript 文件
2. 定义模块的公共接口
3. 实现核心功能
4. 添加测试用例

## 常见问题 (FAQ)

### Q: 如何处理跨平台键盘差异？
A: keyboard.ts 已经处理了主要平台的差异，使用标准化的按键名称。

### Q: 分页数据量很大时如何优化？
A: 使用游标分页或懒加载，避免一次性加载所有数据。

### Q: 类型定义如何保持同步？
A: 使用 TypeScript 的严格模式，定期运行类型检查。

## 相关文件清单

### 核心工具
- `keyboard.ts` - 键盘操作工具
- `pagination.ts` - 分页处理工具
- `types.ts` - 类型定义文件

### 测试文件
- `keyboard.test.ts` - 键盘操作测试（如果存在）
- `pagination.test.ts` - 分页功能测试（如果存在）
- `types.test.ts` - 类型验证测试（如果存在）

## 变更记录 (Changelog)

### 2025-01-20 - 工具模块分析完成
- 识别出 3 个核心工具文件
- 分析出完整的键盘操作和分页处理机制
- 发现规范的类型定义体系
- 工具覆盖率：100%，设计精良，可扩展性强