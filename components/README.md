# 运行组件归档说明（Releases 托管）

自本版本起，以下 4 类大体积运行组件包（共 5 份归档）统一托管于 **GitHub Releases** 与 **GitCode Releases**，不再直接提交进 Git 仓库历史：

- `vertical-grid-egm96-1.0.0.zip`（EGM96 15' 全球大地水准面，全平台通用）
- `vertical-grid-egm2008-1.0.0.zip`（EGM2008 2.5' 高精度大地水准面，全平台通用）
- `native-gis-runtime-1.0.0.zip`（Native GIS GDAL/PROJ 引擎，macOS Apple Silicon `macos-aarch64`）
- `native-gis-runtime-1.0.0-windows-x86_64.zip`（Native GIS GDAL/PROJ 引擎，Windows x64 `windows-x86_64`）
- `compat-worker-1.0.0.zip`（Compatibility Worker 隔离工作进程，仅 Windows x64 `windows-x86_64`）

客户端通过仓库根目录的 `registry.json` 获取各组件的版本、SHA-256 校验值与发行版附件直链；默认优先从 GitCode Releases 国内加速下载，失败时自动回退至 GitHub Releases，并严格执行 64 位小写 SHA-256 校验、体积上限保护与失败自动回滚。
