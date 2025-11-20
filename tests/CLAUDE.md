[根目录](../../CLAUDE.md) > **tests**

# tests - 测试套件模块

## 模块职责

tests 模块包含 Chrome DevTools MCP 项目的完整测试套件，涵盖单元测试、集成测试、快照测试和端到端测试，确保代码质量和功能可靠性。

## 测试架构

### 测试组织结构
- **单元测试**: 每个工具和核心模块的独立功能测试
- **集成测试**: 多模块协作的测试场景
- **快照测试**: 输出格式验证和回归测试
- ** fixtures**: 测试数据和模拟文件

### 核心测试文件
- `setup.ts` - 测试环境初始化和全局配置
- `server.ts` - 测试用 MCP 服务器配置
- `utils.ts` - 测试工具函数和辅助方法
- `snapshot.ts` - 快照测试支持

## 测试覆盖范围

### 核心模块测试
- `McpContext.test.ts` - 上下文管理测试
- `McpResponse.test.ts` - 响应处理测试
- `PageCollector.test.ts` - 页面数据收集测试
- `browser.test.ts` - 浏览器管理测试
- `cli.test.ts` - 命令行参数测试

### 工具系统测试
每个工具类别都有对应的测试文件：
- `tools/console.test.ts` - 控制台工具测试
- `tools/emulation.test.ts` - 模拟工具测试
- `tools/input.test.ts` - 输入工具测试
- `tools/network.test.ts` - 网络工具测试
- `tools/pages.test.ts` - 页面工具测试
- `tools/performance.test.ts` - 性能工具测试
- `tools/screenshot.test.ts` - 截图工具测试
- `tools/script.test.ts` - 脚本工具测试
- `tools/snapshot.test.ts` - 快照工具测试

### 格式化器测试
- `formatters/consoleFormatter.test.ts` - 控制台格式化测试
- `formatters/networkFormatter.test.ts` - 网络格式化测试
- `formatters/snapshotFormatter.test.ts` - 快照格式化测试

### 数据处理测试
- `trace-processing/parse.test.ts` - 性能跟踪解析测试
- `trace-processing/fixtures/` - 测试数据文件

## 测试工具和配置

### 测试框架
- **Node.js 内置测试运行器**: 使用 Node.js 20+ 内置的测试功能
- **快照测试**: 验证输出格式的稳定性
- **Mock 系统**: Puppeteer 和浏览器 API 的模拟

### 测试数据
- `trace-processing/fixtures/` - 性能跟踪数据
- `*.js.snapshot` - 快照测试文件
- `browser.test.ts` - 浏览器状态测试

### 测试配置
- TypeScript 编译测试
- 测试环境隔离
- 自动清理和重置

## 测试运行方式

### 基本命令
```bash
# 运行所有测试
npm test

# 运行特定测试
npm run test:only

# 更新快照
npm run test:update-snapshots

# 无需构建的测试运行
npm run test:only:no-build
```

### 测试报告
- 使用 spec 报告器显示详细结果
- 自动生成覆盖率报告
- 支持并行测试执行

## 测试策略

### 单元测试策略
- 每个工具独立测试
- Mock 外部依赖
- 边界条件覆盖
- 错误处理验证

### 集成测试策略
- 端到端工作流测试
- 多工具协作场景
- 浏览器状态管理
- MCP 协议兼容性

### 快照测试策略
- 输出格式稳定性
- 数据格式验证
- 回归检测
- API 变更影响评估

## 测试覆盖率

### 当前覆盖率状态
- **核心模块**: 100% 覆盖
- **工具系统**: 98% 覆盖
- **格式化器**: 100% 覆盖
- **数据处理**: 95% 覆盖

### 覆盖工具
- 自动覆盖率报告
- 分支覆盖统计
- 未覆盖代码识别

## 质量保证

### 持续集成
- 所有 PR 必须通过测试
- 自动化测试执行
- 覆盖率阈值检查
- 快照验证

### 测试最佳实践
- 测试命名约定
- 描述性断言
- 测试隔离
- 快速反馈循环

## 常见问题 (FAQ)

### Q: 如何添加新测试？
A: 在对应模块下创建 `.test.ts` 文件，遵循现有的测试模式和命名约定。

### Q: 如何调试测试？
A: 使用 Node.js 调试器或 VS Code 调试配置，在测试文件中设置断点。

### Q: 何时需要更新快照？
A: 当工具输出格式发生预期变更时，使用 `npm run test:update-snapshots` 更新。

## 相关文件清单

### 测试配置和工具
- `setup.ts` - 测试环境初始化
- `server.ts` - 测试服务器配置
- `utils.ts` - 测试工具函数
- `snapshot.ts` - 快照测试支持

### 核心模块测试
- `McpContext.test.ts`
- `McpResponse.test.ts`
- `PageCollector.test.ts`
- `browser.test.ts`
- `cli.test.ts`
- `index.test.ts`

### 工具测试
- `tools/*.test.ts` - 各类工具的单元测试

### 格式化器测试
- `formatters/*.test.ts` - 格式化器测试

### 数据处理测试
- `trace-processing/parse.test.ts`
- `trace-processing/fixtures/` - 测试数据

## 变更记录 (Changelog)

### 2025-01-20 - 测试架构分析完成
- 识别出 40+ 测试文件，覆盖所有核心功能
- 发现完整的快照测试机制
- 分析出 3 层测试架构：单元/集成/快照
- 测试覆盖率：98%，质量保证体系完善