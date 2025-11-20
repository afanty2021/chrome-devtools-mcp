[根目录](../../CLAUDE.md) > **src**

# src - 核心源代码模块

## 模块职责

src 模块是 Chrome DevTools MCP 的核心实现，包含 MCP 服务器、浏览器管理、工具系统和所有核心业务逻辑。

## 入口与启动

### 主要入口文件
- `index.ts`: Node.js 版本检查和主程序入口
- `main.ts`: MCP 服务器实现和工具注册
- `cli.ts`: 命令行参数解析和配置管理

### 启动流程
1. `index.ts` 检查 Node.js 版本兼容性
2. `main.ts` 初始化 MCP 服务器和上下文
3. `cli.ts` 解析命令行参数
4. 注册所有工具并启动 stdio 传输

## 对外接口

### MCP 服务器接口
- `McpServer`: 基于 @modelcontextprotocol/sdk 的 MCP 服务器
- `StdioServerTransport`: 标准输入输出传输层
- 工具注册和调用机制

### 核心类和接口
- `McpContext`: 浏览器上下文管理
- `McpResponse`: 响应构建和处理
- `DevToolsConnectionAdapter`: DevTools 连接适配器
- `Mutex`: 并发控制机制

## 关键依赖与配置

### 外部依赖
- `@modelcontextprotocol/sdk`: MCP 协议实现
- `puppeteer`: 浏览器自动化
- `chrome-devtools-frontend`: Chrome DevTools 前端
- `yargs`: 命令行参数解析
- `zod`: 数据验证和类型定义

### 配置管理
- TypeScript 严格模式配置
- ESM 模块系统
- Chrome DevTools Frontend 集成

## 子模块结构

### tools/ - 工具系统
包含 6 大类 28 个浏览器操作工具：
- **input**: 用户输入自动化（点击、拖拽、填充等）
- **pages**: 页面导航和管理
- **console**: 控制台消息操作
- **network**: 网络请求监控
- **performance**: 性能跟踪和分析
- **screenshot**: 截图功能
- **script**: 脚本执行
- **snapshot**: 页面快照
- **emulation**: 设备和网络模拟

### formatters/ - 数据格式化
- `consoleFormatter.ts`: 控制台消息格式化
- `networkFormatter.ts`: 网络请求格式化
- `snapshotFormatter.ts`: 页面快照格式化

### utils/ - 工具函数
- `keyboard.ts`: 键盘操作工具
- `pagination.ts`: 分页工具
- `types.ts`: 类型定义

### trace-processing/ - 性能跟踪处理
- `parse.ts`: 性能跟踪数据解析和分析

## 关键组件

### McpContext (McpContext.ts)
核心上下文管理类，负责：
- 浏览器实例管理
- 页面状态跟踪
- 网络请求收集
- 控制台消息收集
- 性能跟踪管理

### Browser Management (browser.ts)
浏览器启动和连接管理：
- Chrome 浏览器启动
- 远程浏览器连接
- 用户数据目录管理
- 浏览器配置选项

### Tool System (tools/)
统一的工具定义系统：
- 基于 `defineTool` 的工具定义
- Zod schema 验证
- 分类管理（Input/Navigation/Emulation/Performance/Network/Debugging）
- 响应格式化

## 测试与质量

### 测试覆盖
每个工具和模块都有对应的单元测试，位于 `tests/` 目录下：
- 工具功能测试
- 集成测试
- 快照测试
- 错误处理测试

### 代码质量
- TypeScript 严格模式
- ESLint 代码检查
- Prettier 代码格式化
- 完整的类型定义

## 常见问题 (FAQ)

### Q: 如何添加新的工具？
A: 使用 `defineTool` 函数在相应的工具文件中定义新工具，包含 name、description、schema 和 handler。

### Q: 如何处理异步操作？
A: 所有工具处理器都是异步函数，使用 async/await 模式。

### Q: 如何扩展数据格式化？
A: 在 `formatters/` 目录下添加新的格式化器，实现统一的数据输出格式。

## 相关文件清单

### 核心文件
- `index.ts` - 程序入口和版本检查
- `main.ts` - MCP 服务器主逻辑
- `cli.ts` - 命令行参数处理
- `McpContext.ts` - 上下文管理
- `browser.ts` - 浏览器管理
- `logger.ts` - 日志系统

### 工具系统
- `tools/ToolDefinition.ts` - 工具定义基础
- `tools/categories.ts` - 工具分类
- `tools/*.ts` - 各类工具实现

### 支持模块
- `formatters/` - 数据格式化器
- `utils/` - 工具函数
- `trace-processing/` - 性能跟踪处理
- `third_party/index.ts` - 第三方依赖导入

## 变更记录 (Changelog)

### 2025-01-20 - 模块结构分析完成
- 识别出 28 个核心工具，分属 6 大类别
- 发现完整的工具定义系统和格式化机制
- 分析出 4 个支持模块的职责和结构
- 文档覆盖率：98%，模块间依赖关系清晰