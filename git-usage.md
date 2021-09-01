## 基本命令

1. 配置用户名和邮箱账号

   ```bash
   git config –global user.name "username"
   git config –global user.email "email"
   ```

   使用ssh-keygen -t rsa -C "your_email@example.com"生成ssh key。

   添加完ssh key 使用ssh -T git@github.com  检验一下才会生效。

2. 查看远程仓库名称

   ```bash
   git remote
   ```

3. 关联本地仓库与远程仓库

   ```bash
   git remote add origin(远程库的名字) git@github.com:shizhihuaxu/test.git
   git remote set-url origin url 切换远程仓库的地址
   ```

4. 克隆已有仓库

   ```bash
   git clone git@github.com:shizhihuaxu/test.git
   
   //  clone 指定分支
   git clone -b branchname git@github.com:shizhihuaxu/test.git
   ```

5. 列出已有分支

   ```bash
   git branch  列出本地所有分支
   git branch -r 列出所有远程分支
   git branch -a 列出所有分支，包括远程分支和本地分支
   ```

6. 新建分支

   ```bash
   git branch dev			新建分支后不会切换到新分支上
   git checkout -b dev		 新建一个分支并切换到新分支上 	
   git checkout -b new-branch origin/remote-branch		新建一个分支并与远程的某个分支建立关联
   git checkout -b new-branch start-branch  以 start-branch 为原分支创建一个新分支，并切换分支 
   ```

7. 切换分支

   ```bash
   git checkout branch-name	
   ```

8. 删除分支

   ```bash
   git branch -d branch-name
   git branch -D branch-name  强制删除
   git push origin :remote-branch  删除远程分支（省略了local-branch参数）git 1.5.0
   git push origin --delete remote-branch  // git v1.7.0
   ```

9. 合并分支

   ```bash
   git merge another-branch target-branch   如果合并到当前操作分支，target-branch 参数可省略
   git merge --abort 退出合并
   git reset --hard HEAD 取消merging 状态
   git merge --ff  当使用 fast-forward 合并时，不创建新的 commit 节点
   git merge --no-ff  即使使用 fast-forward 合并，也要创建一个新的 commit 节点
   git 
   ```

10. 添加修改到暂存区

    ```
    git add filename		 添加某个文件
    git add .				添加所有文件
    git add -u 				只添加改动过的文件
    ```

11. 暂存代码与重新获取

    ```
    git stash
    git stash pop
    ```

12. commit  提交修改

    ```
    // -m 参数
    git commit -m 'some description'  指定提交描述信息
    
    // -a 参数
    只提交改动
    
    // --amend 参数
    会合并当前提交和上一次的提交，如果当前提交有注释，则以当前的注释为合并后的提交的注释，若当前提交没有注释，则以上一次提交的注释作为合并后的提交的注释。
    ```

13. push 将本地的某个分支提交到远程分支

    ```bash
    git push origin local-branch:remote-branch
    
    //  -u 参数在第一次推送分支时将本地分支与远程分支建立关联，如果远程的分支不存在，会新建
    git push -u origin local-branch:remote-branch 
    相当于
    git push origin local-branch:remote-branch
    git branch --set-upstream-to=origin/remote-branch local-branch
    
    //  未指定 local-branch,表示删除远程的某个分支 
    git push origin  :remote-branch
    ```

14. fetch 拉取远程分支上的代码

    ```
    // 没有任何参数会下载所有的提交记录到相应的远程分支
    git fetch 
    
    //  不会建立映射关系，如果本地分支不存在，则新建，新建后不会自动切换到新分支
    git fetch origin remote-branch:local-branch
    
    //  未指定remote-branch 表示新建一个本地分支
    git fetch origin :local-branch
    ```

15. pull  拉取并合并

    ```
    git pull origin remote-branch:local-branch
    相当于
    git fetch origin remote-branch:local-branch
    git merge local-branch
    ```

    

16. 删除远程仓库上的文件
    ```
    git rm  --cached file_path/file_name
    git commit -m '删除说明'
    git push
    ```

17. 打标签
    ```bash
    # 给指定 commit 打 tag
    git tag -a tagname commit_id -m '说明信息'
    -a 用于指定标签名
    -m 用于添加说明
    commit_id 为打标签的 id
    
    # 直接为分支的最后一次提交打 tag
    git tag tagname
    ```

18. 推送到远程

    ```bash
    # 推送所有的 tag 更改，包括标签的新增与删除修改
    git push origin --tags 
    
    # 只推送单个tag
    git push origin tagname
    ```

    

19. 查看 tag 列表

    ```bash
    git tag
    ```

    

20. 删除本地 tag 

    ```bash
    git tag -d tagname
    ```

    

21. 删除远程分支上的 tag

    ```bash
    git push origin :refs/tags/tagname
    ```

    


## 使用情景

1. 撤销 add 后但没有commit (可能是push)的文件修改

   ```bash
   git checkout --filename width extension-name
   git reset HEAD <file>
   ```

2. 丢弃本地所有修改，未

   ```bash
   git checkout .
   ```

3. 查看本地分支与远程分支的关联关系

   ```
   git remote show origin
   或
   git branch -vv
   ```
   
4. 建立追踪关系的几种方式

   ```bash
   git branch -u origin/remote-branch local-branch
   git branch --set-upstream-to origin/remote-branch local-branch
   
   // 在第一次推送某个本地分支时创建
   git push -u origin local-branch:remote-branch 
   ```

5. 取消追踪关系

   ```bash
   git branch --unset-upstream  local-branch  会取消本地分支与上游分支的关联
   ```

6. 克隆一个仓库后拉取所有远程分支到本地，并且使本地分支与远程分支建立关联

   ```
   git checkout -b local-branch origin/remote-branch
   
   或 
   
   git checkout -b local-branch
   git branch -u origin/remote-branch local-branch
   ```

   

7. 只合并一个分支的部分 commit 到另一个分支

   ```
   // 合并一个 commit
   git cherry-pick commit_id // 在当前分支选中目标分支的某次提交
   
   
   // 合并多个 commit A~C 的所有提交
   git cherry-pick commit_A commit_B commit_C
   git cherry-pick commit_A..Commit_C  
   ```

   

8. 撤销 commit

   撤销后保留代码的修改

   ```
   git reset commit_id 或
   git reset HEAD~1
   ```

   撤销后也丢弃代码修改，加上 --hard 参数

   ```
   git reset --hard HEAD~1
   ```

   

9. 撤销已经 push 的代码

   ```
   git revert HEAD  // 创建一个新的提交作为修改上一次的提交
   git reset --keep commit_id 不会保留原来错误的提交记录，并且之前改动的文件会保存在工作目录中
   git reset --hard commit_id 不保留改动的文件
   // 紧接着将远程的错误commit 删除掉
   git push -f
   ```

   

10. 查看提交线

    ```
    gitk
    ```

    		每个点代表一次提交，线代表父子关系，而彩色的方块则用来标示一个个引用。 黄点表示 HEAD，红点表示尚未提交的本地变动。
    
11. 文件名称大小写写错，git 默认不敏感，不会认为是两个文件

    ```
    // 在项目构建之初就设置大小写敏感
    git config core.ignorecase false
      
    // 对于已经提交到了远程的，先 cd 到所在的子文件夹
    git mv old_name new_name
    ```
    
12. 为分支添加注释工具 git-br

    ```
    npm i -g git-br  // 全局安装
    
    git br // 查看所有分支描述
    
    git config branch.分支名.description '描述内容 // 添加描述
    ```
13. 撤销已经 commit 后的某个文件

    ```
    // 查看这个文件的 commit 记录，找到回退到某个状态的 commit_id
    git log [path of file] 
    
    // 切换到此文件的那个 commit 的状态
    git checkout [commit_id] [path of file] 
    
    // 重新添加本次调整
    git add [path of file]
   
    // 更新提交
    git commit --amend
    ```
