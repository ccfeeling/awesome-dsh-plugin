<!-- Thanks for contributing! Quick checklist / 提交前快速自查 -->

- [x] I added **one file** at `data/plugins/<owner>__<repo>.yml` — that single file is the whole submission. The READMEs are regenerated on `main` after merge: don't edit them by hand, and you don't need to commit them either / 我新增了一个 `data/plugins/<owner>__<repo>.yml` 文件——**这一个文件就是全部投稿**。README 会在合并后由 `main` 自动重新生成：不要手工编辑，也不需要提交
- [x] My repo's `package.json` declares **`dsh.bundle`** (not just `dsh.client`) / 仓库 `package.json` 已声明 `dsh.bundle`
- [ ] My repo is at least **1 day old** — the repository was created today (2026-11-04), so the submission gate's one-day floor reaches it tomorrow. The gate re-runs on its own, and nothing else about this submission depends on the age / 仓库建于今天（2026-11-04），仓库年龄的 1 天下限明天自动达标，gate 会自行重跑
- [x] `category` is one of `agi ui usage theme model identity session memory tools wsl browser vision voice docs skill workflow git notify dev security remote market fun`, and themes/skins go under `theme` / `category` 取值正确
- [x] Description states what the plugin does, no superlatives / 描述只说功能，不带营销词
- [x] My repo has the `dsh-plugin` topic / 仓库已打 `dsh-plugin` topic

**Recommended (not required) / 推荐但不强制：**

- 📦 Publish to npm — not done: the package name is a local scope, so the prebuilt tarball route is used instead / 未发布 npm：包名是本地 scope，改用预构建 tarball
- 🔗 Declare official `@deepseek-ai/*` packages as `peerDependencies` — not applicable: the plugin depends on no `@deepseek-ai/*` package at all / 不适用：本插件不依赖任何 `@deepseek-ai/*` 包
- 🖼️ Screenshots go in **your own repository**: a `screenshots.json` beside your `package.json` — done / 截图放在仓库自己的 `screenshots.json` 里：已完成

## What it does / 插件做什么

A left-sidebar panel that browses the filesystem of the machine the Harness runs on:

- directory listings with kinds, sizes and modification times, hidden entries dimmed;
- text file previews (the first 256 KiB);
- inline image previews (PNG, JPEG, GIF, WebP, AVIF, SVG);
- file downloads, handed to the browser through the Harness's own authenticated file route, under the file's own name;
- file uploads into the directory that is open — one Remote call per file, carrying the bytes as canonical base64, never replacing a file that is already there;
- folder creation;
- twelve languages: every visible string goes through `ctx.locale`, and the panel adds its languages to the Harness language list.

Apart from those two writes the plugin is read-only: it cannot modify, delete or rename an existing file.

The Host half is a `ctx.hostFiles` service over `node:fs/promises`, exposed on the Remote transport; the Client half is a Remote contribution that registers a `sidebar.panellist` entry and a `main` panel. It has no dependencies and no build step, so the attached tarball is exactly what the repository contains.

## Install / 安装

```bash
dsh plugin --profile web add github:ccfeeling/dsh-host-file-explorer
```

or let the marketplace offer the prebuilt tarball attached to the repository's release.
