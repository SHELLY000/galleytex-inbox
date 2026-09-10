# inbox — 把稿件放在这里 / drop manuscripts here

放进这个目录并提交的文件会被 GitHub Actions 自动处理，本机不需要装任何软件：

| 放入 | 处理 | 得到 |
|---|---|---|
| `xxx.docx`（作者的 Word 稿） | GalleyTeX 自动转换 → XeLaTeX 编译 | `project.zip`、`ui_report.json`（识别结果与检查清单）、`xxx.pdf`、`main.log` |
| `xxx.zip`（在 GalleyTeX 网页里校对后导出的项目） | XeLaTeX 编译 | `xxx.pdf`、`main.log` |

会议/期刊信息在仓库根目录的 `venue.json` 里（与网页"导出会议设置"得到的文件相同），改一次全批生效。

## 批量流程

1. 一次拖 10 个文件进来（网页：本目录 → **Add file → Upload files**），一次 Commit。
2. 到 **Actions** 页面等 "GalleyTeX inbox" 跑完（10 篇约 5 分钟）。
3. 点进本次运行，**Summary** 里有一张表：每篇的检查清单警告数、页数、LaTeX 错误数、结果。
4. 页面底部 **Artifacts → galleytex-output** 下载，每篇一个目录。
5. 检查清单警告为 0、编译无错的稿件可以直接用；有警告的（由加粗推断的标题、无法转换的公式对象、缺国家等），
   在 GalleyTeX 网页里打开原 Word 逐块校对，导出 ZIP，再放回本目录编译。

## 注意

- 文件名用英文数字（如 `2026-0012-zhang.docx`），避免中文和空格。
- 每次 push 只处理这次新增或修改的文件；Actions 页面点 **Run workflow** 会重新处理目录里的全部文件。
- 自动转换使用 acmsmall 单栏、`acmlicensed` 版权声明；需要投稿（manuscript）格式的稿件请在网页里导出后放 ZIP。
- 稿件会进入这个仓库，仓库必须是 **私有** 的；处理完的文件请删除。
