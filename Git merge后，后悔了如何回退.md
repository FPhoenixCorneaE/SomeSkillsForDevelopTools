### 步骤

#### 1.git checkout到你要恢复的那个分支上

​	git checkout develop

#### 2.git reflog查出要回退到merge前的版本号

​	git reflog

#### 3.git reset --hard [版本号]就回退到merge前的代码状态了

​	git reset --hard f82cfd2