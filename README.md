# WordTeX inbox — 编辑部批量转换与编译

把作者的 Word 稿件放进 `inbox/`，GitHub Actions 自动完成 **Word → LaTeX（acmart）→ XeLaTeX → PDF**，
本机不需要安装 TeX、Node 或任何软件。转换器来自公开仓库
[SHELLY000/wordtolatexZL](https://github.com/SHELLY000/wordtolatexZL)，每次运行自动拉取并构建。

> 稿件会进入这个仓库。**仓库必须保持私有**，只把编辑部同事加为协作者。

## 一次性设置

1. `venue.json` 就是 WordTeX 网页第一步"会议资料"表单的文件形式，格式与网页里 **导出会议设置** 按钮生成的文件相同：
   在网页里把会议信息填全、点导出，把得到的 JSON 改名为 `venue.json` 放到仓库根目录即可（也可以直接编辑）。
   当前文件已预填公开版应用默认的会议名，但日期、地点、ISBN 是空的，需要补上。`lang` 决定检查清单语言（`zh` / `en`）。
2. `.github/workflows/wordtex-inbox.yml` 顶部的 `APP_REF` 决定用哪个版本的转换器：
   `main` 跟最新，或写标签如 `v0.2.2` 固定版本。
3. 推送本仓库。`inbox/test-sample.docx` 是一篇示例稿，第一次推送会自动跑一遍，
   在 Actions 页面看到 ✅ 就说明流水线通了，然后把它删掉。

## 日常使用

| 放进 `inbox/` | 自动处理 | 产出（每篇一个目录） |
|---|---|---|
| `xxx.docx` 作者的 Word 稿 | WordTeX 转换 → XeLaTeX | `project.zip`、`ui_report.json`、`xxx.pdf`、`main.log` |
| `xxx.zip` 网页校对后导出的项目 | XeLaTeX | `xxx.pdf`、`main.log` |

1. 一次可以放 10 篇甚至更多：本目录 → **Add file → Upload files** → 拖入 → Commit（或 `git add inbox && git commit && git push`）。
2. **Actions** 页面等 "WordTeX inbox" 跑完，10 篇约 5 分钟。
3. 点进本次运行，**Summary** 里有汇总表：每篇的检查清单警告数、页数、LaTeX 错误数、结果。
4. 页面底部 **Artifacts → wordtex-output** 下载全部产物。
5. 分诊：警告 0 且编译无错的直接用；有警告的在 WordTeX 网页里打开原 Word 逐块校对，导出 ZIP 放回 `inbox/` 再编译。

细节见 [`inbox/README.md`](inbox/README.md)。

## 出了问题看哪里

- 运行页面顶部 **Annotations** 直接列出每篇的 LaTeX 错误行。
- 产物目录里的 `convert-output.txt`（转换过程）、`latexmk-output.txt`（编译过程）、`main.log`（完整 TeX 日志）。
- 每次 push 只处理这次新增或修改的文件；要重跑全部，Actions 页面点 **Run workflow**。

## 费用与保留

- 每次运行约 3 分钟固定开销（装 TeX + 构建转换器）+ 每篇约 10 秒。
- 私有仓库的 Actions 免费额度按月计（免费账户约 2000 分钟），够日常使用；大批量时把一批文件放在同一次提交里最省。
- 产物保留 30 天；处理完的稿件请从 `inbox/` 删除，避免仓库膨胀。
