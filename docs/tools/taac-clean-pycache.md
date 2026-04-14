# Python缓存清理 

## 目标

`taac-clean-pycache` 用于批量清理仓库中的 `__pycache__` 目录，减少无用缓存占用，并避免提交或检索时被噪声文件干扰。

## 命令入口

```bash
uv run taac-clean-pycache
```

脚本实现位置：`src/taac2026/application/maintenance/clean_pycache.py`。

## 默认行为

1. 默认扫描仓库根目录。
2. 默认跳过常见环境目录：`.venv`、`venv`、`env`、`.tox`、`node_modules` 等。
3. 对每个匹配目录执行删除，并输出 `[removed] <path>`。
4. 结束时输出摘要统计：匹配目录数、处理目录数、文件数、体积、失败数等。

## 参数说明

| 参数                 | 类型   | 默认值     | 说明                                                                   |
| -------------------- | ------ | ---------- | ---------------------------------------------------------------------- |
| `--root`             | string | 仓库根目录 | 指定扫描起点                                                           |
| `--dry-run`          | flag   | `false`    | 仅预览将删除的目录，不实际删除                                         |
| `--include-env-dirs` | flag   | `false`    | 连 `.venv`、`venv`、`env`、`.tox`、`node_modules` 等环境目录也一起扫描 |

## 推荐用法

```bash
# 1) 先预览
uv run taac-clean-pycache --dry-run

# 2) 确认后执行删除
uv run taac-clean-pycache

# 3) 指定目录
uv run taac-clean-pycache --root outputs

# 4) 连环境目录一并扫描（谨慎）
uv run taac-clean-pycache --include-env-dirs
```

## 输出示例

```text
[removed] F:\path\to\TAAC_2026\src\taac2026\__pycache__
root=F:\path\to\TAAC_2026
mode=delete
matched_dirs=1
processed_dirs=1
matched_files=8
matched_size_mib=0.0342
include_env_dirs=False
failures=0
```

## 返回码

1. `0`：执行成功。
2. `1`：执行完成但存在删除失败项。
3. `2`：参数错误（例如 `--root` 不存在或不是目录）。

## 注意事项

1. 建议始终先用 `--dry-run` 预览。
2. 只有在明确需要时再开启 `--include-env-dirs`。
3. 在 CI 中可根据返回码判定步骤是否失败。
