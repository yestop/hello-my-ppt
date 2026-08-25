# hello-my-ppt Git 同步说明

本目录是工作树，Git 元数据放在工作区旁边的：

```text
git-repos/hello-my-ppt.git
```

这是因为当前 Codex 工作区禁止在 skill 目录内创建 `.git` 隐藏目录。代码内容和普通 Git 仓库完全一致，只是命令需要同时指定 `--git-dir` 和 `--work-tree`。

## 查看状态

PowerShell：

```powershell
$meta = "E:\\Cloud\\具身智能\\调研\\git-repos\\hello-my-ppt.git"
$repo = "E:\\Cloud\\具身智能\\调研\\tools\\hello-my-ppt"
git --git-dir=$meta --work-tree=$repo status
```

## 提交更新

确认改动后，仅暂存明确需要提交的路径：

```powershell
git --git-dir=$meta --work-tree=$repo add -- SKILL.md README.md README.en.md GIT-SYNC.md agents bin editor lib tests
git --git-dir=$meta --work-tree=$repo commit -m "Update hello-my-ppt skill"
```

## 同步到 GitHub

远程地址已经配置为：

```text
https://github.com/yestop/hello-my-ppt.git
```

仓库在 GitHub 建立后，首次推送：

```powershell
git --git-dir=$meta --work-tree=$repo push -u origin main
```

