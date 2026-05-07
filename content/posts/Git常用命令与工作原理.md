---
title: "Git常用命令与工作原理"
date: 2026-05-07
draft: false
tags: ["Git"]
categories: ["技术"]
---

## 前言

大家好！今天给大家分析关于Git版本管理的知识，主要包括什么是Git、Git常用命令和Git的工作原理。

## 什么是Git

在写作、编程或是任何需要反复修改文件的过程中，你大概率都经历过这样的场景：改着改着突然想回头，却找不到几天前的那个绝妙版本了；或者和伙伴协作一份文稿，文件传来传去，根本分不清哪个才是最终版。

这时候，就需要一个能帮你记住每一次修改、自由回溯、还能多人并肩协作的工具——这就是 Git。

Git 是一种分布式版本控制系统。我们可以把它想象成一套精密的“游戏存档”加“平行宇宙”系统。每当你在项目中完成一个小阶段，就可以主动“存档”一下（在 Git 里叫一次提交）。这个存档不是简单覆盖，而是完整记录下此刻所有文件的状态、是谁改的、什么时候改的以及为什么这样改。将来不管是想回到三天前的某个状态，还是对比两次修改间的差异，都只需一条命令。

和传统的集中式版本控制不同，Git 的“分布式”意味着每个人的电脑上都有一份完整的历史记录仓库。这带来了两个巨大好处：一是绝大部分操作都在本地瞬间完成，不用总得联网；二是每一份本地仓库都是一个完整备份，谁电脑上的数据丢了都没关系，从同伴那儿拷贝一份就能继续工作。

Git 里还有一个极为强大的概念叫分支。你可以把它理解成从主线故事里开辟出一条独立的“副本时间线”。想尝试一个疯狂的新功能？建一个分支随便折腾，改坏了也不会影响主项目；等实验成功，再把它合并回主线就好。这种“低成本试错”的方式，让敏捷开发和多人并行协作成为了现实。

## Git工作原理

Git 的精髓在于，它并不记录文件每次改动的差异，而是把每一次提交都当作整个项目的完整快照保存下来。没改动的文件，它就聪明地复用之前相同内容的引用，只对真正变化的部分创建新对象。更巧妙的是，所有内容——无论是文件、目录还是提交记录——都被存成对象，并用内容本身的 SHA-1 哈希值作为唯一标识。这意味着内容相同必然指纹相同，任何篡改都无处遁形，历史的完整与可信从此被数学保证。

在你日常使用中，文件会经历三个区域：工作区是你实际编辑文件的地方，暂存区像待发货的包裹，通过 git add 把想要保留的修改放入其中，最后用 git commit 将暂存区变成仓库里一枚永久的快照。而所谓的分支，不过是一个指向某次提交的轻量指针。新建分支只是瞬间创建一个新指针，切换分支只是移动 HEAD 的指向，合并分支更是把两个提交并到同一祖先下，生成一次新快照。正因为没有笨重的文件复制，只有指针的轻巧移动，Git 才能在毫秒间完成版本跃迁，让你随时低成本地试验任何想法。

![Git工作原理](/static/images/Git/Git工作流程图.png)

## Git常用命

在这里我会给大家分享我在日常学习工作中常用的Git命令，这些命令也是日常工作流中最基础最常用的命令。

### 创建仓库

```bash
# 在当前目录初始化
git init

# 创建新目录并初始化
git init project-name

# 克隆远程仓库
git clone <url>

# 克隆到指定目录
git clone <url> <directory>

# 克隆指定分支
git clone -b <branch> <url>
```

### 文件操作

#### 查看状态

```bash
# 查看工作区状态
git status

# 简洁模式
git status -s
```

#### 添加文件

```bash
# 添加指定文件
git add <file>

# 添加所有文件
git add .
```

#### 移动和删除

```bash
# 移动/重命名文件
git mv <old> <new>

# 删除文件（同时从工作区删除）
git rm <file>

# 删除文件（仅从暂存区删除）
git rm --cached <file>

# 删除目录
git rm -r <directory>

```

#### 撤销和修改

```bash
# 撤销工作区修改
git restore <file>
# 或
git checkout -- <file>

# 撤销暂存
git restore --staged <file>
# 或
git reset HEAD <file>

# 撤销所有暂存和工作区修改
git restore --staged --worktree <file>
# 或
git checkout HEAD -- <file>
```

### 提交操作

#### 创建提交

```bash
# 提交暂存区内容
git commit

# 带消息提交
git commit -m "commit message"

# 提交并添加详细描述
git commit -m "title" -m "description"
```

#### 提交规范

推荐使用 [Conventional Commits](https://www.conventionalcommits.org/) 规范：

**类型（type）**：
| 类型 | 说明 |

|------|------|
| `feat` | 新功能 |
| `fix` | 修复 Bug |
| `docs` | 文档更新 |
| `style` | 代码格式（不影响逻辑） |
| `refactor` | 重构 |
| `perf` | 性能优化 |
| `test` | 测试相关 |
| `chore` | 构建/工具相关 |
| `ci` | CI/CD 相关 |

**示例**：

```bash
git commit -m "feat(auth): add OAuth2 login support"
git commit -m "fix(api): resolve null pointer exception"
git commit -m "docs: update installation guide"
```

### 分支管理

#### 创建分支

```bash
# 查看分支
git branch

# 查看远程分支
git branch -r

# 创建分支
git branch <branch-name>

# 创建并切换分支
git switch -c <branch-name>
# 或
git checkout -b <branch-name>
```

#### 切换分支

```bash
# 切换分支
git switch <branch-name>
# 或
git checkout <branch-name>
```

#### 删除分支

```bash
# 删除已合并的分支
git branch -d <branch-name>

# 强制删除分支
git branch -D <branch-name>

# 删除远程分支
git push origin --delete <branch-name>
```

#### 重命名分支

```bash
# 重命名当前分支
git branch -m <new-name>

# 重命名指定分支
git branch -m <old-name> <new-name>
```

### 历史查看

#### 查看提交历史

```bash
# 查看历史
git log

# 单行显示
git log --oneline

# 图形化显示
git log --graph --oneline --all

# 显示最近 N 条
git log -n 5
```

#### 查看文件历史

```bash
# 查看文件修改历史
git log -- <file>

# 查看文件修改历史（含重命名）
git log --follow -- <file>

# 查看文件每次提交的具体修改
git log -p -- <file>

# 查看谁修改了文件
git blame <file>

# 查看指定行的修改
git blame -L 10,20 <file>
```

#### 查看特定提交

```bash
# 查看提交详情
git show <commit>

# 查看提交的文件变更
git show --stat <commit>

# 查看提交的某个文件
git show <commit>:<file>
```

### 远程仓库

#### 管理远程仓库

```bash
# 查看远程仓库
git remote

# 查看详细信息
git remote -v

# 添加远程仓库
git remote add origin <url>

# 修改远程仓库 URL
git remote set-url origin <new-url>

# 删除远程仓库
git remote remove <name>

# 重命名远程仓库
git remote rename <old> <new>
```

#### 推送代码

```bash
# 推送到远程
git push origin <branch>

# 推送并设置上游分支
git push -u origin <branch>

# 推送所有分支
git push --all origin

# 推送标签
git push --tags

# 强制推送（谨慎使用）
git push --force-with-lease
```

#### 拉取代码

```bash
# 拉取并合并
git pull origin <branch>

# 拉取并变基
git pull --rebase

# 仅获取不合并
git fetch origin

# 获取所有远程分支
git fetch --all

# 获取并清理已删除的远程分支
git fetch -p
```

---

### 合并与变基

#### 合并分支

```bash
# 合并指定分支到当前分支
git merge <branch>

# 合并并压缩
git merge --squash <branch>

# 取消合并
git merge --abort

# 查看合并状态
git status
```

#### 变基操作

```bash
# 变基到指定分支
git rebase <branch>

# 交互式变基
git rebase -i HEAD~5

# 继续 rebase
git rebase --continue

# 跳过当前提交
git rebase --skip

# 取消 rebase
git rebase --abort
```

#### 解决冲突

```bash
# 查看冲突文件
git status

# 使用编辑器解决冲突后
git add <resolved-file>

# 继续合并/rebase
git commit  # 合并时
git rebase --continue  # rebase 时

# 使用特定版本
git checkout --ours <file>      # 使用当前分支版本
git checkout --theirs <file>    # 使用合并分支版本
```

#### Cherry-pick

```bash
# 应用指定提交
git cherry-pick <commit>

# 应用多个提交
git cherry-pick <commit1> <commit2>

# 应用提交范围
git cherry-pick <start>..<end>

# 仅应用但不提交
git cherry-pick -n <commit>
```

---

### 撤销与回退

#### 撤销提交

```bash
# 撤销最近一次提交（保留修改）
git reset HEAD^

# 撤销最近一次提交（丢弃修改）
git reset --hard HEAD^

# 撤销到指定提交
git reset <commit>

# 软重置（保留暂存区和工作区）
git reset --soft <commit>

# 混合重置（保留工作区）
git reset --mixed <commit>

# 硬重置（全部丢弃）
git reset --hard <commit>
```

#### 回退提交

```bash
# 创建新提交来撤销
git revert <commit>

# 撤销多个提交
git revert <commit1>..<commit2>

# 撤销但不自动提交
git revert -n <commit>
```

#### 恢复操作

```bash
# 查看操作历史
git reflog

# 恢复到指定状态
git reset --hard HEAD@{n}

# 恢复已删除的提交
git cherry-pick <commit>
```

### 标签管理

#### 创建标签

```bash
# 创建轻量标签
git tag <tag-name>

# 创建附注标签
git tag -a <tag-name> -m "message"

# 为指定提交创建标签
git tag <tag-name> <commit>

# 创建签名标签
git tag -s <tag-name> -m "message"
```

#### 管理标签

```bash
# 查看所有标签
git tag

# 查看标签详情
git show <tag-name>

# 删除本地标签
git tag -d <tag-name>

# 删除远程标签
git push origin --delete tag <tag-name>

# 推送标签到远程
git push origin <tag-name>

# 推送所有标签
git push --tags
```

#### 检出标签

```bash
# 检出标签
git checkout <tag-name>

# 基于标签创建分支
git checkout -b <branch-name> <tag-name>
```