[根目录](../../../CLAUDE.md) > [src](../../) > **tools**

# tools - 浏览器操作工具系统

## 模块职责

tools 模块是 Chrome DevTools MCP 的核心功能实现，提供 6 大类 28 个浏览器操作工具，涵盖用户输入、页面导航、性能分析、网络监控、设备模拟和调试功能。

## 工具架构

### 工具定义系统
所有工具使用统一的 `defineTool` 函数定义：
```typescript
export const toolName = defineTool({
  name: 'tool_name',
  description: '工具描述',
  annotations: {
    category: ToolCategory.CATEGORY,
    readOnlyHint: boolean,
  },
  schema: ZodSchema,
  handler: async (request, response, context) => {
    // 工具逻辑
  }
});
```

### 工具分类
- **INPUT**: 输入自动化工具（8个）
- **NAVIGATION**: 导航自动化工具（7个）
- **EMULATION**: 模拟工具（3个）
- **PERFORMANCE**: 性能工具（3个）
- **NETWORK**: 网络工具（2个）
- **DEBUGGING**: 调试工具（5个）

## 工具详细清单

### 1. Input Automation (输入自动化)
**文件**: `input.ts`

| 工具名 | 功能描述 | 只读 | 说明 |
|--------|----------|------|------|
| `click` | 点击页面元素 | ❌ | 支持多种定位策略 |
| `drag` | 拖拽元素 | ❌ | 支持源目标和目标位置 |
| `fill` | 填充输入框 | ❌ | 自动等待元素可用 |
| `fill_form` | 填充表单 | ❌ | 批量表单字段处理 |
| `handle_dialog` | 处理对话框 | ❌ | 接受、拒绝或输入文本 |
| `hover` | 悬停元素 | ❌ | 触发悬停事件 |
| `press_key` | 按键操作 | ❌ | 支持组合键 |
| `upload_file` | 文件上传 | ❌ | 支持多文件上传 |

### 2. Navigation Automation (导航自动化)
**文件**: `pages.ts`

| 工具名 | 功能描述 | 只读 | 说明 |
|--------|----------|------|------|
| `list_pages` | 列出所有页面 | ✅ | 显示浏览器标签页 |
| `navigate_page` | 页面导航 | ❌ | 跳转到指定 URL |
| `navigate_page_history` | 历史导航 | ❌ | 前进/后退操作 |
| `new_page` | 新建页面 | ❌ | 创建新标签页 |
| `select_page` | 选择页面 | ❌ | 切换活动页面 |
| `close_page` | 关闭页面 | ❌ | 关闭指定页面 |
| `wait_for` | 等待条件 | ✅ | 等待元素或文本 |

### 3. Emulation (设备模拟)
**文件**: `emulation.ts`

| 工具名 | 功能描述 | 只读 | 说明 |
|--------|----------|------|------|
| `emulate_cpu` | CPU 模拟 | ❌ | 设置 CPU 节流倍率 |
| `emulate_network` | 网络模拟 | ❌ | 预设网络条件 |
| `resize_page` | 页面尺寸调整 | ❌ | 设置视口大小 |

### 4. Performance (性能分析)
**文件**: `performance.ts`

| 工具名 | 功能描述 | 只读 | 说明 |
|--------|----------|------|------|
| `performance_start_trace` | 开始性能跟踪 | ✅ | 记录页面性能数据 |
| `performance_stop_trace` | 停止性能跟踪 | ✅ | 分析性能指标 |
| `performance_analyze_insight` | 性能洞察分析 | ✅ | 提供优化建议 |

### 5. Network (网络监控)
**文件**: `network.ts`

| 工具名 | 功能描述 | 只读 | 说明 |
|--------|----------|------|------|
| `list_network_requests` | 列出网络请求 | ✅ | 支持分页和过滤 |
| `get_network_request` | 获取网络请求详情 | ✅ | 请求头、响应等信息 |

### 6. Debugging (调试工具)
**文件**: `console.ts`, `screenshot.ts`, `script.ts`, `snapshot.ts`

| 工具名 | 功能描述 | 只读 | 说明 | 文件 |
|--------|----------|------|------|------|
| `list_console_messages` | 列出控制台消息 | ✅ | 支持消息类型过滤 | console.ts |
| `get_console_message` | 获取控制台消息详情 | ✅ | 根据ID获取特定消息 | console.ts |
| `take_screenshot` | 页面截图 | ✅ | 支持多种格式 | screenshot.ts |
| `evaluate_script` | 执行脚本 | ❌ | 在页面上下文中执行 | script.ts |
| `take_snapshot` | 页面快照 | ✅ | 可访问性树结构 | snapshot.ts |

## 核心组件

### ToolDefinition.ts
工具系统的基础定义：
- `ToolDefinition` 接口：工具结构定义
- `Request/Response` 接口：请求响应模型
- `Context` 接口：工具执行上下文
- `defineTool` 函数：工具创建辅助

### categories.ts
工具分类定义：
- `ToolCategory` 枚举：6 个工具类别
- `labels` 对象：类别显示名称映射

## 工具执行流程

### 1. 工具注册
```typescript
// 在 main.ts 中注册所有工具
const tools = [
  ...Object.values(consoleTools),
  ...Object.values(emulationTools),
  // ... 其他工具类别
];
tools.forEach(tool => registerTool(tool));
```

### 2. 工具调用
1. MCP 客户端发送工具调用请求
2. 服务器验证参数（Zod schema）
3. 创建 `McpContext` 和 `McpResponse`
4. 执行工具 `handler` 函数
5. 处理响应并返回结果

### 3. 并发控制
- 使用 `Mutex` 确保工具串行执行
- 避免浏览器状态冲突
- 提供操作原子性保证

## 数据验证和类型安全

### Zod Schema 验证
每个工具定义严格的输入 schema：
```typescript
schema: {
  selector: zod.string().describe('CSS选择器'),
  timeout: timeoutSchema.timeout, // 复用超时定义
  waitForSelector: zod.boolean().optional().describe('是否等待选择器')
}
```

### 通用 Schema 定义
- `timeoutSchema`: 标准超时参数
- 分页参数：用于列表类工具
- 过滤参数：用于数据筛选

## 测试策略

### 单元测试
- 每个工具独立的 `.test.ts` 文件
- Mock 浏览器 API
- 边界条件测试
- 错误处理验证

### 快照测试
- 验证工具输出格式
- 确保向后兼容性
- 检测意外的格式变更

## 常见问题 (FAQ)

### Q: 如何添加新工具？
A: 在相应类别的文件中使用 `defineTool` 创建新工具，包含 name、description、annotations、schema 和 handler。

### Q: 工具间如何共享状态？
A: 通过 `McpContext` 对象管理共享状态，如当前页面、网络请求收集器等。

### Q: 如何处理异步操作？
A: 所有工具 handler 都是 async 函数，使用 await 处理 Puppeteer 异步操作。

## 相关文件清单

### 核心定义
- `ToolDefinition.ts` - 工具定义基础
- `categories.ts` - 工具分类

### 工具实现
- `input.ts` - 输入自动化工具
- `pages.ts` - 页面导航工具
- `console.ts` - 控制台工具
- `network.ts` - 网络监控工具
- `performance.ts` - 性能分析工具
- `screenshot.ts` - 截图工具
- `script.ts` - 脚本执行工具
- `snapshot.ts` - 页面快照工具
- `emulation.ts` - 模拟工具

## 变更记录 (Changelog)

### 2025-01-20 - 工具系统分析完成
- 识别出 28 个工具，分属 6 大类别
- 分析出完整的工具定义和注册机制
- 发现统一的并发控制和数据验证系统
- 工具覆盖率：100%，分类清晰，设计规范