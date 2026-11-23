# EOStage Extension Hub (`eostage-hub`)

简体中文 | [English](README.en.md)

[EOStage](https://github.com/hhanmj/EOStage) 的官方扩展清单与按需分发仓库，用于托管大体积运行时组件、遥感与 GEE 知识库插件包及工作流技能。

## 仓库结构

- `registry.json`：统一扩展索引（Schema v2），记录下载链接或相对路径、适用平台、体积与 64 位小写 SHA-256 校验值。
- `blocklist.json`：吊销清单，支持按 `id` 或 `sha256` 拦截问题或安全风险版本。
- `components/`：运行组件说明目录（大体积压缩包统一托管于 `v1.1.0` Releases，不在 Git 历史中堆积二进制文件）。
- `plugins/`：遥感与 GEE 知识库插件包（`.zip`）。
- `skills/`：独立工作流提示词技能清单（`.json`）。

## 当前收录（16 项）

### 运行组件（4 项 · 共 5 个 Releases 归档）

| ID | 版本 | 适用平台 | 说明 |
| --- | --- | --- | --- |
| `vertical-grid-egm96` | `1.0.0` | 全平台 | NGA 15' 全球大地水准面高程基准网格（EPSG:5773） |
| `vertical-grid-egm2008` | `1.0.0` | 全平台 | NGA 2.5' 高精度全球大地水准面网格（EPSG:3855） |
| `native-gis-runtime` | `1.0.0` | `Windows x64` / `macOS Apple Silicon` | GDAL / PROJ 私有动态库与投影基准环境（双平台分包） |
| `compat-worker` | `1.0.0` | `windows-x86_64` | 仅 Windows x64 使用的兼容隔离工作进程 |

### 知识库插件包（4 项 · 全平台）

| ID | 版本 | 协议 | 说明 |
| --- | --- | --- | --- |
| `eostage-eo-recipes-plugin` | `1.1.0` | `Apache-2.0` | 遥感与生态水文分析配方库（83 项 GEE Python + 16 项本地 GeoTIFF） |
| `eostage-gee-cookbook-plugin` | `1.1.0` | `Apache-2.0` | GEE 遥感与地学云端计算教材攻略（5 章系统手册） |
| `eostage-gee-practice-plugin` | `1.0.0` | `Apache-2.0` | GEE 科研实践与避坑知识卡（10 项专题规程） |
| `eostage-eo-methodology-plugin` | `1.0.0` | `Apache-2.0` | 遥感建模验证、归因审查与工作流路由技能包（4 项） |

### 工作流技能（8 项 · 全平台）

| ID | 版本 | 说明 |
| --- | --- | --- |
| `skill-alos2-stripmap-insar` | `1.0.0` | ALOS-2 PALSAR-2 L 波段植被穿透形变监测工作流 |
| `skill-nisar-gslc-subset-prep` | `1.0.0` | NISAR L/S 双频 GSLC 远程子集裁剪与准备工作流 |
| `skill-gacos-tropospheric-delay` | `1.0.0` | GACOS 对流层大气延迟校正配对工作流 |
| `skill-eo-agent-capability-routing` | `1.0.0` | 遥感智能体意图与工具匹配 |
| `skill-eo-model-explanation-review` | `1.0.0` | 遥感模型归因与误差解释 |
| `skill-eo-model-transfer-evaluation` | `1.0.0` | 遥感模型跨区域评估与选型 |
| `skill-eo-workbench-concept` | `1.0.0` | 遥感工作台概念设计 |
| `skill-gee-practice-cards` | `1.0.0` | GEE 科研实践与工程避坑知识卡 |

## 清单与插件格式

### `registry.json` 条目格式

```json
{
  "id": "my-eo-plugin",
  "name": "示例遥感插件",
  "kind": "plugin",
  "version": "1.0.0",
  "author": "Author",
  "license": "Apache-2.0",
  "category": "insar-sar",
  "description": "简短说明插件用途",
  "package": "plugins/my-eo-plugin-1.0.0.zip",
  "sha256": "<64-char-lowercase-sha256>",
  "size": 12345,
  "install_root": "plugins/my-eo-plugin",
  "platforms": [
    "windows-x86_64",
    "macos-aarch64",
    "macos-x86_64",
    "linux-x86_64",
    "linux-aarch64"
  ]
}
```

- `id`、`name`、`kind`（`component` / `plugin` / `skill`）、`version`、`package`（相对路径或官方 Releases HTTPS 地址）、`sha256`、`size`、`platforms`（或分平台 `platform_packages`）为核心字段。
- 客户端下载后强制核验 `size` 与 `sha256`；校验失败不会覆盖既有安装目录。

### 插件包目录与 `plugin.json`

标准知识库/技能插件压缩包内采用相对目录结构（禁止路径穿越与系统隐藏文件）：

```text
plugin.json
skills/<skill-id>/SKILL.md
skills/<skill-id>/references/*.md
```

`plugin.json` 最小示例：

```json
{
  "id": "my-eo-plugin",
  "name": "示例遥感插件",
  "version": "1.0.0",
  "author": "Author",
  "license": "Apache-2.0",
  "discipline": "insar-sar",
  "description": "简短说明",
  "bundledSkills": [
    {
      "id": "my-skill",
      "name": "示例技能",
      "description": "触发条件与用途说明",
      "path": "skills/my-skill/SKILL.md"
    }
  ]
}
```

### `blocklist.json` 吊销格式

```json
{
  "version": 1,
  "updated_at": "2026-11-09T07:42:12Z",
  "blocked": [
    {
      "id": "bad-plugin",
      "sha256": "<可选：仅拦截特定版本>",
      "reason": "向用户展示的拦截原因"
    }
  ]
}
```

## 镜像与客户端使用

- **双端自动镜像**：主库与 `v1.1.0` Releases 大包托管于 GitHub，通过 `.github/workflows/sync-mirror.yml` 自动将代码、标签与 Releases 附件同步至 GitCode；客户端默认拉取 GitCode 国内源，失败时自动回退至 GitHub 官方源。
- **客户端入口**：在 EOStage 桌面端打开 **「插件与技能」** 同步扩展技能与插件（支持 **「本地导入 .zip」**），或打开 **「设置 -> 运行环境与工具」** 按需安装高程基准网格与本地 GIS 运行组件（支持在线安装与 **「从本地包导入安装」**）。

## 贡献与问题反馈

- **提交新插件/技能**：在遵守对应开源协议且不含个人路径、凭据或恶意代码的前提下，通过 Pull Request 或 Issue 提交，经维护者核验内容与 SHA-256 后纳入官方清单。
- **故障与风险反馈**：发现链接失效、校验不匹配或安全隐患，请在仓库提交 Issue 说明插件 `id` 与现象。
