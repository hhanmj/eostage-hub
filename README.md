# EOStage Extension Hub (`eostage-hub`)

本仓库用于托管 EOStage 的按需安装大体积运行组件包（高程基准网格、地理信息引擎、兼容工作进程），使主安装包保持轻量化；同时兼作遥感与 GEE 知识库技能的远端备份。

> **架构说明**：所有遥感与 GEE 分析配方知识库及方法学技能已直接内置于 EOStage 桌面端本地包内，无需额外占用自有服务器带宽；GitCode 与 GitHub 仅负责大体积运行组件的按需分发与双端同步备份。

## 目录结构

- `registry.json`：统一扩展清单（Schema v2，采用相对路径 `package`、分平台 `platforms` / `platform_packages` 与强制 64 位小写 SHA-256 校验值，支持 GitCode / GitHub 双端零修改同步）
- `blocklist.json`：安全吊销列表（支持按 `id` 或 `sha256` 拦截问题版本）
- `components/`：按需下载的运行组件压缩包（客户端下载后执行 SHA-256 校验、`.old` 原子备份、自检与失败自动回滚）
- `skills/`：扩展工作流提示词技能 JSON 备份
- `plugins/`：遥感分析配方与建模方法学知识库压缩包备份

## 当前收录清单（共 13 项）

### 1. 运行组件（4 项，严格区分支持系统）

| 组件 ID | 说明 | 适用系统（`platforms`） | 备注 |
| --- | --- | --- | --- |
| `vertical-grid-egm96` | NGA 15' 全球大地水准面高程基准网格（EPSG:5773） | 全平台（Windows x64 / macOS / Linux） | 通用数据网格 |
| `vertical-grid-egm2008` | NGA 2.5' 高精度全球大地水准面网格（EPSG:3855） | 全平台（Windows x64 / macOS / Linux） | 通用数据网格 |
| `native-gis-runtime` | GDAL / PROJ 私有动态库闭包与投影基准数据库 | `macos-aarch64`（Apple Silicon） | Windows x64 包待在 Windows 环境编译后补充 |
| `compat-worker` | 传统格式与兼容交付隔离工作进程 | `windows-x86_64`（Windows x64） | 仅适用于 Windows x64 |

### 2. 工作流提示词技能备份（7 项，全平台通用）

- `skill-alos2-stripmap-insar`：ALOS-2 PALSAR-2 L 波段植被穿透形变监测工作流
- `skill-nisar-gslc-subset-prep`：NISAR L/S 双频 GSLC 远程子集裁剪与准备工作流
- `skill-gacos-tropospheric-delay`：GACOS 对流层大气延迟校正配对工作流
- `skill-eo-agent-capability-routing`：遥感智能体意图与工具匹配
- `skill-eo-model-explanation-review`：遥感模型归因与误差解释
- `skill-eo-model-transfer-evaluation`：遥感模型跨区域评估与选型
- `skill-eo-workbench-concept`：遥感工作台概念设计

### 3. 遥感知识库插件包备份（2 项，全平台通用，核心内容已内置于 EOStage）

- `eostage-eo-recipes-plugin`：**遥感与生态水文分析配方知识库**（27 项 GEE Python 云端算子 + 16 项本地 GeoTIFF 窗口流式配方，共 43 项标准算子）
- `eostage-eo-methodology-plugin`：**遥感建模验证、归因审查与工作流路由技能包**（4 项方法学技能与核验规程）

## 双端同步部署（GitHub + GitCode）

1. **推送至 GitHub 与 GitCode 双端仓库**：
   ```bash
   git remote add github https://github.com/hhanmj/eostage-hub.git
   git remote add gitcode https://gitcode.com/hhanmj/eostage-hub.git
   git push -u github main
   git push -u gitcode main
   ```
   > **提示**：GitHub Actions（`.github/workflows/sync-mirror.yml`）已配置推送后自动镜像同步；EOStage 客户端在直连 API 限流或预览不可用时会自动走 GitCode 标准 Git 协议通道拉取大组件并回退至 GitHub。

2. **客户端切换镜像源**：
   - 打开 EOStage 桌面端 **「设置」 -> 「运行环境与工具」**；
   - 可切换 **GitCode 国内加速源（自动含免限流通道）**、**GitHub 官方源** 或 **本地扩展清单**，所选配置会在重启后自动保留。
