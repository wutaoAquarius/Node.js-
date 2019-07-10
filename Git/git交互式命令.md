## 配置交互式命令的编辑器为vscode

```sh
git config --global core.editor "code --wait"
```

## 交互式命令

```sh
git rebase -i commit的提交id
```

### root截止到第一次提交

```sh
git rebase -i --root
```

## 查看操作历史记录

```sh
git reflog
```

使用 `git reset --hard reflog的Id` 恢复操作记录
