[根目录](../../CLAUDE.md) > **scripts**

# scripts - 构建和开发工具

## 模块职责

scripts 模块包含项目的构建脚本、开发工具、文档生成器和维护脚本，确保项目的自动化构建、代码质量检查和文档维护。

## 脚本分类

### 1. 构建脚本
- `prepare.ts` - 项目准备和环境检查
- `post-build.ts` - 构建后处理和优化

### 2. 文档生成
- `generate-docs.ts` - 自动生成工具参考文档

### 3. 开发工具
- `verify-server-json-version.ts` - 版本号验证
- `sed.ts` - 文本处理工具

### 4. 代码质量
- `eslint_rules/` - 自定义 ESLint 规则

## 核心脚本详解

### generate-docs.ts
**功能**: 自动从工具定义生成文档

**主要特性**:
- 扫描所有工具定义
- 提取工具描述和参数 schema
- 生成 Markdown 格式的工具参考
- 自动更新 README.md 中的工具列表

**使用方式**:
```bash
npm run docs:generate
```

### post-build.ts
**功能**: TypeScript 编译后处理

**主要任务**:
- 清理构建输出
- 优化 bundle 大小
- 处理第三方依赖
- 生成最终分发包

### prepare.ts
**功能**: 项目准备脚本

**检查项目**:
- Node.js 版本兼容性
- 依赖完整性检查
- 环境变量设置
- 构建目录准备

### verify-server-json-version.ts
**功能**: 版本一致性检查

**验证内容**:
- package.json 版本
- server.json 版本
- 源代码中的硬编码版本
- 确保发布版本一致性

## 代码质量工具

### ESLint 自定义规则
**目录**: `eslint_rules/`

#### check-license-rule.js
- 检查文件头部的 Apache-2.0 许可证声明
- 确保所有源文件包含正确的许可证信息
- 支持多种文件类型的许可证格式

#### local-plugin.js
- 本地 ESLint 插件定义
- 集成自定义规则
- 提供项目特定的代码检查

### 构建配置
- 使用 Rollup 进行打包优化
- Tree-shaking 和代码压缩
- 第三方依赖处理
- 输出格式标准化

## 脚本执行流程

### 完整构建流程
```bash
npm run build
# 1. clean: 清理构建目录
# 2. tsc: TypeScript 编译
# 3. post-build: 构建后处理
# 4. rollup: 打包优化
```

### 文档生成流程
```bash
npm run docs
# 1. build: 编译项目
# 2. docs:generate: 生成工具文档
# 3. format: 格式化文档
```

### 开发流程
```bash
npm run prepare        # 环境准备
npm run verify-server-json-version  # 版本验证
npm run format         # 代码格式化
```

## 自动化集成

### CI/CD 集成
- 构建脚本在 CI 环境中自动执行
- 版本检查确保发布一致性
- 代码质量检查集成到 PR 流程

### 发布流程
- 版本号自动更新
- 构建产物自动优化
- 文档自动生成和更新

## 开发者工具

### 调试支持
- 构建过程日志
- 错误详细信息
- 性能统计报告

### 本地开发
- 增量构建支持
- 文件监听和自动重建
- 开发环境优化

## 配置文件

### TypeScript 配置
- `scripts/tsconfig.json` - 脚本专用 TypeScript 配置
- 独立的编译选项
- 针对脚本的优化设置

### 工具集成
- ESLint 配置集成自定义规则
- Prettier 格式化配置
- 构建工具链优化

## 常见问题 (FAQ)

### Q: 如何添加新的构建步骤？
A: 在 `post-build.ts` 中添加新的处理逻辑，或创建独立的脚本文件。

### Q: 自定义 ESLint 规则如何工作？
A: 规则定义在 `eslint_rules/` 目录，通过 `local-plugin.js` 集成到主 ESLint 配置。

### Q: 文档生成如何定制？
A: 修改 `generate-docs.ts` 中的模板和处理逻辑，适应不同的文档需求。

## 相关文件清单

### 主要脚本
- `generate-docs.ts` - 文档生成器
- `post-build.ts` - 构建后处理
- `prepare.ts` - 项目准备
- `verify-server-json-version.ts` - 版本验证
- `sed.ts` - 文本处理工具

### 代码质量
- `eslint_rules/check-license-rule.js` - 许可证检查规则
- `eslint_rules/local-plugin.js` - 本地 ESLint 插件

### 配置
- `tsconfig.json` - TypeScript 配置

## 变更记录 (Changelog)

### 2025-01-20 - 脚本系统分析完成
- 识别出 6 个核心脚本，覆盖构建、文档、质量检查
- 发现完整的代码质量工具链
- 分析出自动化文档生成机制
- 脚本覆盖率：100%，工具链完善，自动化程度高