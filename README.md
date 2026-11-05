# EOStage Extension Hub (`eostage-hub`)

本仓库用于托管 EOStage 的按需安装运行组件（高程基准网格、兼容工作进程、扩展运行时）与官方扩展提示词技能（Skills），以保持 EOStage 主仓库轻量化与纯净。

## 目录结构

- `registry.json`：统一扩展清单（采用相对路径 `package` 与强制 64 位小写 SHA-256 校验值，支持多镜像源零修改同步）
- `blocklist.json`：安全吊销列表（支持按 `id` 或 `sha256` 拦截问题版本）
- `components/`：按需下载的运行组件压缩包（客户端下载后执行 SHA-256 校验、`.old` 原子备份、自检与失败自动回滚）
- `skills/`：可扩展的 InSAR / 遥感专业工作流提示词技能 JSON

## 多源免费加速镜像部署指南（GitCode + GitHub + Cloudflare Free）

1. **推送至 GitHub 与 GitCode 双端仓库**：
   ```bash
   cd _hub_dist
   git init
   git branch -M main
   git add .
   git commit -m "chore(hub): initial eostage-hub registry and components"
   git remote add github https://github.com/hhanmj/eostage-hub.git
   git remote add gitcode https://gitcode.com/hhanmj/eostage-hub.git
   git push -u github main
   git push -u gitcode main
   ```
   > **提示**：若单个 `.zip` 超过平台 Git 仓库单文件限制（如 100 MB），可将大体积 `.zip` 上传至对应仓库的 **Releases** 附件或 Cloudflare R2（免费 10 GB 存储且免出口流量费），并将 `registry.json` 对应条目的 `package` 指向对应的路径。

2. **客户端切换镜像源**：
   - 打开 EOStage 桌面端 **「插件与技能」 -> 「运行组件 (Runtime)」** 页签；
   - 可一键切换 **GitCode 国内加速源**、**Cloudflare 免费镜像**、**GitHub 官方源** 或 **本地清单 (`_hub_dist/registry.json`)**。
