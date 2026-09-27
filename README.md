# Blender 创作辅助插件（dsh-blender-helper）

给 **DSH（DeepSeek Harness）** 用的 Blender 创作辅助插件：让 AI 助手**直接读你正开着的 Blender、并且改它**——
声明式生成/修改模型、出预览图、按你此刻的界面状态教你怎么操作、按需联网搜素材、MMD 工作流（PMX/VMD）、只读的模型拓扑分析。

## 下载

到 [**Releases**](https://github.com/GA17-LHY/dsh-blender-helper/releases) 下载**最新版**（当前 **v0.1.1**）：

- 文件：`Blender-helper-plugin-v0.1.1-win.zip`
- SHA256：`c51294f86359705c0dc9ad327e772dfc74e95769633346e1a7b73e4344b8186a`

```powershell
Get-FileHash .\Blender-helper-plugin-v0.1.1-win.zip -Algorithm SHA256
```

> ⚠️ **不要使用 v0.1.0**（已撤回）：那版的三个安装脚本是 UTF-8 **无 BOM**，在中文 Windows 上会被
> Windows PowerShell 5.1 按 ANSI(GBK) 读，中文注释撑坏引号而**根本无法运行**；`安装到DSH.ps1`
> 还可能写坏你的 `cordis.patch.yml`。v0.1.1 已修复。

## 包里有什么（两半，都要装）

| 目录 | 装到哪 | 作用 |
|---|---|---|
| `dsh侧插件\` | dsh | 提供 8 个 `blender_*` 工具；含 `安装到DSH.ps1` |
| `blender侧插件\` | Blender | 让助手能读改你正开着的场景；含 `安装到Blender.ps1` / `卸载.ps1` |
| `给DSH的安装说明.md` | —— | 可执行 runbook（前置条件、手工步骤、排错表、回滚） |
| `给创作者的注意事项.md` | —— | 给使用者看的：能力、写权限、最容易吃亏的几件事 |

只装一半会出现两种典型症状：只装 dsh 侧 → 实时功能报「live 未连接」；只装 Blender 侧 → 有面板但没人给它发指令。

## 前置条件

- Windows 10 / 11
- **Node.js 20+（建议 22+）**
- **pnpm**（必需：`dsh plugin add` 内部就是 pnpm 的转发器）
- **dsh**
- **Blender 4.2+**（本包在 5.0.1 上实测）
- 可选：**MMD Tools** 扩展（只有 PMX/VMD 相关功能需要）

## 安装

1. 解压到**不含空格、最好是纯英文**的目录
2. `cd <解压目录>\dsh侧插件` → `powershell -ExecutionPolicy Bypass -File .\安装到DSH.ps1`
3. **重启 dsh**
4. 让助手执行 `blender_live action=doctor` 验收

Blender 侧：`cd <解压目录>\blender侧插件` → `powershell -ExecutionPolicy Bypass -File .\安装到Blender.ps1`，
然后**重启 Blender** → `编辑 → 偏好设置 → 插件` 搜「创作助手」启用 → 3D 视图按 `N` → 侧栏「创作助手」→ 点「启动创作助手服务」。

## 已知边界

- **写权限默认只读**：要允许写，只能在 Blender 侧栏 N 面板点「允许写操作」（助手自己开不了）。被拒时它会明说，不会假装成功。
- 依赖按打包机器的宿主版本固化（`@deepseek-ai/dsh-tools` 等）。目标机 dsh 版本差异较大而加载失败时，
  在 `dsh侧插件\` 里跑 `pnpm install` 重新解析依赖，再重启 dsh。
- Blender 4.2~5.x 应当可用（manifest 声明最低 4.2.0），但**只在 5.0.1 实测过**。
- 服务只监听 `127.0.0.1`，不对外开放端口。
- 三个安装脚本都是 **UTF-8 带 BOM**（这是必须的：脚本含中文注释，而 PowerShell 5.1 在中文系统上
  会把无 BOM 的 UTF-8 按 GBK 读；删掉 BOM 会让脚本无法运行）。
- 发布前已对包内内容做过个人信息扫描；包内不含发布者的用户名、账号或本机绝对路径。