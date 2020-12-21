**假设来源为：**  `https://github.com/_original/_project.git`
**fork 项目为：** `https://github.com/_your/_project.git`



### 步骤

#### 1.检出自己在github上fork别人的分支到目录下 

​    git clone https://github.com/_your/_project.git

#### 2.进到 _project 目录下，然后增加远程分支(fork的分支)，名为 updateStream（名字任意）到本地

​    git remote add updateStream https://github.com/_original/_project.git

#### 3.运行命令：`git remote -v`, 会发现多出来了一个 updateStream 的远程分支

​    git remote -v

#### 4.然后把远程原始分支 updateStream 的代码拉到本地 

​    git fetch updateStream 

#### 5.合并对方远程原始分支 updateStream 的代码

​    git merge updateStream/master

#### 6.最后把最新的代码推送到你的github上

  git push origin master

#### 7.如果需要给update_stream发送Pull Request

  **打开 `https://github.com/_your/_project.git` 
  点击 Pull Request -> 点击 New Pull Request -> 输入 Title 和功能说明 -> 点击 Send pull request**



### 假如原项目增加了新的分支 production，我们可以通过以下流程将新分支同步下来：

#### 1.获取远程分支内容

​	git fetch updateStream production:production

### 2.切换分支

​	git checkout production

### 3.推送本地分支到 origin

​	git push origin production

### 4.关联本地 production 与远程 production

​	git branch --set-upstream-to=origin/production production