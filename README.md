# EOStage Extension Hub (`eostage-hub`)

本仓库用于托管 EOStage 的扩展清单、工作流技能、知识库插件包，以及通过 **GitHub Releases + GitCode Releases 双端发行版** 分发按需安装的大体积运行组件包（高程基准网格、地理信息引擎、兼容工作进程），使主安装包与 Git 仓库均保持轻量化。

> **架构说明**：
> - 轻量的 `registry.json`、`blocklist.json`、`skills/` 工作流提示词与 `plugins/` 遥感知识库插件包直接托管于 Git 仓库内；
> - 4 类大体积运行组件包（EGM96、EGM2008、macOS Apple Silicon 与 Windows x64 双端 `native-gis-runtime`、Windows x64 `compat-worker`）统一托管于 **GitHub Releases** 与 **GitCode Releases**，由自动化工作流实现“只需上传 GitHub Release，自动同步 GitCode Release”。

## 目录结构

- `registry.json`：统一扩展清单（Schema v2，轻量技能/插件使用相对路径，大体积运行组件使用发行版附件直链、分平台 `platforms` / `platform_packages` 与强制 64 位小写 SHA-256 校验值）
- `blocklist.json`：安全吊销列表（支持按 `id` 或 `sha256` 拦截问题版本）
- `components/`：运行组件发行版托管说明（压缩包附件托管于 Releases，不再直接提交至 Git）
- `skills/`：扩展工作流提示词技能 JSON 备份
- `plugins/`：遥感分析配方与建模方法学知识库压缩包备份

## 当前收录清单（共 16 项）

### 1. 运行组件（4 项，Releases 双端托管，严格区分支持系统）

| 组件 ID | 说明 | 适用系统（`platforms`） | 发行版附件文件名（`v1.1.0`） |
| --- | --- | --- | --- |
| `vertical-grid-egm96` | NGA 15' 全球大地水准面高程基准网格（EPSG:5773） | 全平台（Windows x64 / macOS / Linux） | `vertical-grid-egm96-1.0.0.zip` |
| `vertical-grid-egm2008` | NGA 2.5' 高精度全球大地水准面网格（EPSG:3855） | 全平台（Windows x64 / macOS / Linux） | `vertical-grid-egm2008-1.0.0.zip` |
| `native-gis-runtime` | GDAL / PROJ 私有动态库闭包与投影基准数据库 | `macos-aarch64`（Apple Silicon）、`windows-x86_64`（Windows x64） | `native-gis-runtime-1.0.0.zip`（macOS Apple Silicon）<br>`native-gis-runtime-1.0.0-windows-x86_64.zip`（Windows x64） |
| `compat-worker` | 传统格式与兼容交付隔离工作进程 | `windows-x86_64`（Windows x64） | `compat-worker-1.0.0.zip` |

### 2. 工作流提示词技能备份（8 项，全平台通用）

- `skill-alos2-stripmap-insar`：ALOS-2 PALSAR-2 L 波段植被穿透形变监测工作流
- `skill-nisar-gslc-subset-prep`：NISAR L/S 双频 GSLC 远程子集裁剪与准备工作流
- `skill-gacos-tropospheric-delay`：GACOS 对流层大气延迟校正配对工作流
- `skill-eo-agent-capability-routing`：遥感智能体意图与工具匹配
- `skill-eo-model-explanation-review`：遥感模型归因与误差解释
- `skill-eo-model-transfer-evaluation`：遥感模型跨区域评估与选型
- `skill-eo-workbench-concept`：遥感工作台概念设计
- `skill-gee-practice-cards`：GEE 科研实践与工程避坑知识卡

### 3. 遥感与 GEE 知识库插件包（4 项，全平台通用）

- `eostage-eo-recipes-plugin`（v1.1.0）：**遥感与生态水文分析配方知识库**（83 项 GEE Python 云端算子 + 16 项本地 GeoTIFF 窗口流式配方，共 99 项标准算子，已内置同步）
- `eostage-gee-cookbook-plugin`（v1.1.0）：**GEE 遥感与地学云端计算教材攻略**（5 章架构原理、去云选型、谐波趋势、Olofsson 无偏面积与服务端调优手册，已内置同步）
- `eostage-gee-practice-plugin`（v1.0.0）：**GEE 科研实践与避坑知识卡**（10 项专题卡：去云对照边界、HLS 跨传感器一致化、S1 线性域、时序 `valid_obs`、多源 QA 矩阵、切块安全边界、导出预演路由、数据血缘台账、EECU 配额与任务排错）
- `eostage-eo-methodology-plugin`（v1.0.0）：**遥感建模验证、归因审查与工作流路由技能包**（4 项方法学技能与核验规程，已内置同步）

## 双端同步部署（GitHub + GitCode）

1. **代码与标签镜像同步**：
   - 推送至 GitHub `main` 分支或 `v*` 标签后，`.github/workflows/sync-mirror.yml` 会自动将仓库历史与标签镜像同步至 GitCode（及已配置令牌的 Gitee）。
2. **发行版与大组件附件自动同步**：
   - 在 GitHub 发布或编辑 Release（或在 Actions 中手动触发 `workflow_dispatch` 并填写 `release_tag`）时，`sync-mirror.yml` 会先确保目标标签已推送至 GitCode，随后利用 `GITCODE_TOKEN` 自动在 GitCode 创建/更新同版本 Release，并将 GitHub Release 下的全部组件附件同步上传至 GitCode Releases。
3. **客户端镜像源与自动回退**：
   - EOStage 桌面端默认内置 **GitCode 国内加速源**，并在发行版下载异常或不可达时自动回退至 **GitHub Releases**；
   - 打开 EOStage 桌面端 **「设置」 -> 「运行环境与工具」**，可一键在线安装运行组件或导入本地组件包（底层配置同时保留 GitCode / GitHub / 本地自定义源持久化测试能力）；
   - 全过程强制执行 64 位小写 SHA-256 校验、体积上限保护、跨平台防呆检测、安装后自检与失败自动回滚。
