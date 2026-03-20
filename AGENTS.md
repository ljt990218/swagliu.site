# Overview

- 这是一个基于 Nuxt 3 的个人博客站点，主要内容来自 `content/`，页面路由由 `pages/` 驱动，公共展示组件集中在 `components/`。
- 包管理器使用 `pnpm`，锁文件为 `pnpm-lock.yaml`。
- 站点基础信息和导航配置在 `site.config.ts`，RSS 路由位于 `server/routes/rss.ts`。

# 项目结构

- `pages/`：站点页面与动态路由，例如 `pages/p/[...post].vue`、`pages/tags/[tag].vue`。
- `components/`：通用组件与内容渲染组件，`components/doc/` 下是文档页相关组件。
- `content/`：博客内容源文件，当前分为 `blog/`、`life/`、`record/`，另有 `me.md` 和 `404.md`。
- `assets/styles/`：全局样式、主题样式、Markdown 样式和过渡样式。
- `layouts/`：Nuxt 布局及其子组件。
- `server/routes/`：服务端路由，目前包含 `rss.ts`。
- `utils/`：工具函数目录，改动前先确认是否已有可复用实现。

# 构建和运行命令

- 安装依赖：`pnpm install`
- 本地开发：`pnpm dev`
- 生产构建：`pnpm build`
- 静态生成：`pnpm generate`
- 本地预览：`pnpm preview`
- 代码检查与自动修复：`pnpm lint`
- 样式检查与自动修复：`pnpm stylelint:fix`

# 额外约定

- `nuxt.config.ts` 已固定开发服务为 `0.0.0.0:3333`，本地联调默认使用这个端口。
- 全局样式入口在 `nuxt.config.ts` 的 `css` 字段，不要绕过这里零散注入全局 CSS。
- `tsconfig.json` 直接继承 `./.nuxt/tsconfig.json`，路径别名和 Nuxt 类型以 Nuxt 生成结果为准。
- 项目启用了 `@nuxt/content`、`@unocss/nuxt`、`@vueuse/nuxt` 和 `@nuxtjs/stylelint-module`，新增能力优先沿用现有模块。

# 代码规范

- ESLint 使用 `@chansee97/eslint-config-vue`，并额外关闭了 `vue/dot-location` 规则；改动 Vue/TS/JS 文件后优先运行 `pnpm lint`。
- Stylelint 使用 `@chansee97/stylelint-config`；样式文件和 `.vue` 中样式块改动后优先运行 `pnpm stylelint:fix`。
- 仓库未见独立 Prettier 配置，格式风格以现有文件和 ESLint/Stylelint 自动修复结果为准。
- 保持实现简单直接，沿用现有 Nuxt 约定式目录，不要为小功能引入新的重型抽象。
- 提交前清理无关文件，尤其不要把 `.DS_Store`、临时产物或无关格式化噪音混入提交。

# Git Workflow

- 最近提交风格以 Conventional Commits 为主，例如 `docs(me): ...`、`feat(styles): ...`、`chore: ...`。
- 已配置 `simple-git-hooks` 的 `pre-commit`，会执行 `pnpm lint-staged`。
- `*.{js,jsx,ts,tsx}` 运行 `eslint --fix`
- `*.{css,scss,less,html}` 运行 `stylelint --fix`
- `*.vue` 同时运行 `eslint --fix` 和 `stylelint --fix`
- 上述 `lint-staged` 规则会在提交前自动执行。
- 保持提交粒度小，避免顺手改动无关文件；不要覆盖或回退自己未负责的改动。

# 测试方式

- `package.json` 中没有定义 `test` 脚本，仓库内也没有 `tests/`、Vitest、Jest、Playwright 或 Cypress 配置。
- 当前主要依赖 lint 和本地运行验证改动：至少执行 `pnpm lint`，涉及样式时再执行 `pnpm stylelint:fix`。
- 涉及页面、内容渲染或 RSS 路由的改动，建议通过 `pnpm dev` 手动验证关键页面和接口表现。
