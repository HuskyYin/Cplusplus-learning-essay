## 1.初始化/添加文件
`git init` 用以初始化一个git仓库<br/>
`git add <filename>` 添加文件；可添加多个<br/>
`git add -A` 可以添加仓库中所有变化至缓冲区<br/>
`git commit -m <message>` 提交添加完成<br/>

## 2.查看git仓库状态
`git status` 查看git仓库状态<br/>
1. `Changes not staged for commit:`还有修改没有添加到缓存区<br/>
2. `Untracked files:`仓库目录下还有没有被git追踪的文件<br/>
3. `Changes to be committed:`缓冲区有新添加或修改的文件，待提交<br/>

## 3.版本回退
`git reset --hard commit_id` 回退版本，其中**commit_id**可以指定版本ID，或者用`HEAD^`、`HEAD^^`表示当前版本之前多少个版本<br/>
`git log` 回退以前版本之前查看提交历史，以确定回退到哪个版本<br/>
`git reflog` 回退后重回版本之前查看命令历史，以确定重回到哪个版本<br/>

## 4.暂存区
git暂存区保存工作区执行`git add`后的状态，`git commit`将当前暂存区进行提交（而非当前工作区）。因此，为了将所有改动提交，需要在最后一次改动后进行`git add`然后使用`git status`检查状态，确认无误后再进行`git commit`<br/>

## 5.撤销修改
`git restore`用以撤销修改，分为两种情况：<br/>
1. 如果修改还没有执行`git add`:<br/>
&emsp;&emsp;直接使用`git restore <filename>`可以直接撤销工作区的修改<br/>
2. 如果修改已经执行了`git add`:<br/>
&emsp;&emsp;可以使用`git restore --staged <filename>`可以将暂存区的的修改回退到没有提交的状态,这就还原到了**情况1**.通过进一步执行`git restore`可以撤销工作区修改。

## 6.删除/撤销删除
`rm <filename>`用以删除文件<br/>
`git rm <filename>`添加删除文件,`git commit -m <message>` 提交删除完成<br/>
删除操作的撤销操作参考**5.撤销修改**。在执行完`rm <filename>`后，没有执行`git rm <filename>`之前，可以使用`git checkout  -- <filename>`恢复工作区文件。但是如果已经执行了`git rm <filename>`操作，就不可以从暂存区恢复删除文件了，因为`git checkout`是从暂存区恢复文件，而`git rm`是将删除申请提交到缓存区，执行后缓存区将没有相应文件<br/>
删除操作在**commit**操作后,可以使用`git reset --hard <ID>`进行回退<br/>
