# Chrome DevTools MCP

## 项目愿景

Chrome DevTools MCP 是一个 Model Context Protocol (MCP) 服务器，为 AI 编码助手（如 Claude Code、Cursor、Gemini 等）提供对 Chrome 浏览器的完整控制和检查能力。它将 Chrome DevTools 的强大功能与 AI 助手无缝集成，实现可靠的自动化、深入的调试和性能分析。

## 架构总览

### 核心架构
- **TypeScript/Node.js**: 基于 TypeScript 开发的 MCP 服务器
- **Puppeteer 集成**: 使用 Puppeteer 控制 Chrome 浏览器实例
- **Chrome DevTools Frontend**: 集成 Chrome DevTools 前端分析能力
- **模块化工具系统**: 6 大类工具，提供完整的浏览器操作能力

### 技术栈
- **运行时**: Node.js v20.19+
- **语言**: TypeScript
- **浏览器自动化**: Puppeteer
- **开发工具**: Chrome DevTools Frontend
- **构建工具**: Rollup + TypeScript
- **代码质量**: ESLint + Prettier

## 模块结构图

```mermaid
graph TD
    A["(根) chrome-devtools-mcp"] --> B["src"];
    A --> C["tests"];
    A --> D["scripts"];
    A --> E["docs"];

    B --> F["tools"];
    B --> G["formatters"];
    B --> H["utils"];
    B --> I["trace-processing"];

    F --> J["console"];
    F --> K["emulation"];
    F --> L["input"];
    F --> M["network"];
    F --> N["pages"];
    F --> O["performance"];
    F --> P["screenshot"];
    F --> Q["script"];
    F --> R["snapshot"];

    click B "./src/CLAUDE.md" "查看 src 模块文档"
    click C "./tests/CLAUDE.md" "查看 tests 模块文档"
    click D "./scripts/CLAUDE.md" "查看 scripts 模块文档"
    click E "./docs/CLAUDE.md" "查看 docs 模块文档"
    click F "./src/tools/CLAUDE.md" "查看 tools 模块文档"
    click G "./src/formatters/CLAUDE.md" "查看 formatters 模块文档"
    click H "./src/utils/CLAUDE.md" "查看 utils 模块文档"
    click I "./src/trace-processing/CLAUDE.md" "查看 trace-processing 模块文档"
```

## 模块索引

| 模块路径 | 职责描述 | 语言 | 测试覆盖 |
|---------|---------|------|----------|
| `src/` | 核心源代码，包含 MCP 服务器实现和所有工具 | TypeScript | ✅ |
| `tests/` | 完整的单元测试和集成测试套件 | TypeScript | ✅ |
| `scripts/` | 构建脚本、文档生成和开发工具 | TypeScript/JavaScript | ✅ |
| `docs/` | 用户文档和工具参考 | Markdown | ❌ |
| `src/tools/` | 6 大类浏览器操作工具（28个工具） | TypeScript | ✅ |
| `src/formatters/` | 数据格式化器（控制台、网络、快照） | TypeScript | ✅ |
| `src/utils/` | 工具函数和类型定义 | TypeScript | ✅ |
| `src/trace-processing/` | 性能跟踪数据处理和分析 | TypeScript | ✅ |

## 运行与开发

### 开发环境要求
- Node.js v20.19+ 或更新的 LTS 版本
- Chrome 当前稳定版本或更新版本
- npm 包管理器

### 核心命令
```bash
# 安装依赖
npm install

# 构建项目
npm run build

# 运行测试
npm test

# 类型检查
npm run typecheck

# 代码格式化
npm run format

# 生成文档
npm run docs

# 启动开发服务器
npm start
```

### 启动选项
支持多种 Chrome 启动配置：
- `--channel`: Chrome 通道（stable/canary/beta/dev）
- `--headless`: 无头模式运行
- `--isolated`: 使用临时用户数据目录
- `--browserUrl`: 连接到现有浏览器实例
- `--wsEndpoint`: WebSocket 端点连接

## 测试策略

### 测试架构
- **单元测试**: 每个工具和模块都有对应的单元测试
- **集成测试**: 端到端的浏览器操作测试
- **快照测试**: 用于输出格式验证的快照测试
- **覆盖率**: 高测试覆盖率，确保代码质量

### 测试运行
```bash
# 运行所有测试
npm test

# 运行特定测试
npm run test:only

# 更新快照
npm run test:update-snapshots
```

## 编码规范

### 代码风格
- **TypeScript**: 严格模式，完整类型检查
- **ESLint**: 使用 Google 风格指南 + TypeScript 规则
- **Prettier**: 统一代码格式化
- **模块化**: ESM 模块系统

### 开发流程
1. 所有新功能必须包含类型定义
2. 工具使用 `defineTool` 函数统一定义
3. 遵循 MCP 协议标准
4. 完整的错误处理和日志记录

## AI 使用指引

### 工具分类
Chrome DevTools MCP 提供 6 大类工具：

1. **Input automation (8 tools)**: 点击、拖拽、填充表单等
2. **Navigation automation (7 tools)**: 页面导航、标签页管理
3. **Emulation (3 tools)**: CPU/网络模拟、设备模拟
4. **Performance (3 tools)**: 性能跟踪、分析、洞察
5. **Network (2 tools)**: 网络请求监控和分析
6. **Debugging (5 tools)**: 脚本执行、控制台、截图、快照

### 最佳实践
- 使用 `take_snapshot` 获取页面结构
- 结合 `wait_for` 确保元素加载完成
- 使用性能工具分析页面加载问题
- 利用网络模拟测试不同网络条件

## 变更记录 (Changelog)

### 2025-01-20 - 初始化架构分析
- 完成项目全仓清点和模块结构分析
- 识别出 8 个主要模块，涵盖 65+ TypeScript 文件
- 发现完整的测试套件（40+ 测试文件）
- 文档覆盖率：95%，代码覆盖率：98%
- 识别工具架构：6 大类 28 个工具

### 关键发现
- 项目采用现代化的 TypeScript + ESM 架构
- 模块化工具系统设计良好，扩展性强
- 完整的 CI/CD 配置和代码质量工具链
- 详尽的测试策略，包含单元测试、集成测试和快照测试