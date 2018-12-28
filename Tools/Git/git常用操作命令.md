# git常用操作命令

## 分支

    查看分支：git branch

    创建分支：git branch <name>

    切换分支：git checkout <name>

    创建+切换分支：git checkout -b <name>

    合并某分支到当前分支：git merge <name>

    删除分支：git branch -d <name>

## bug分支

每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除；但是如果当前的`dev`分支工作还未完成，又要创建新的分支，就可以使用

    git stash

命令，保存当前的工作现场，等到bug分支合并删除后，又回到当前的工作现场。
切换回`dev`分支

    git checkout dev

恢复到之前的工作现场

    git stash apply     // 恢复后，stash内容并不删除，你需要用git stash drop来删除；

    git stash pop       // 恢复的同时把stash内容也删了

    git stash apply stash@{0} // 恢复到指定的stash

注：当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。