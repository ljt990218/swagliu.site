[swagliu](https://swagliu.netlify.app/)  
This is my personal website  

Thank them 🙏  
site template from [chansee97](https://github.com/chansee97/nuxt-blog)  
reference project from [antfu.me](https://github.com/antfu/antfu.me)

## Netlify 部署

这个项目已经添加了根目录 [`netlify.toml`](/Users/apple/Desktop/GitHub/swagliu.site/netlify.toml)，直接把仓库上传到 GitHub 后接入 Netlify 即可。

- Build command: `pnpm build`
- Publish directory: `dist`
- Node version: `20`
- `pnpm` 兼容参数: `PNPM_FLAGS=--shamefully-hoist`

如果是新建站点，Netlify 中选择该 GitHub 仓库后保持以上配置即可，不需要额外再写 `_redirects` 或手动指定 Nuxt preset。
