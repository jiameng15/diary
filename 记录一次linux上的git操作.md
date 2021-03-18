# 记录一次linux上的git操作

### 安装git

linux运行环境: CentOS

```
yum install -y git
```



### 记录账号

```
git config --global user.name "username" // 设置用户名

git config --global user.email "abc@email.com" // 设置邮箱

git config --global credential.helper store // 长期保存账号
```





### 分支操作

```
git branch // 查看分支

git status // 查看是否有修改

git checkout -b dev // 创建一个名为dev的本地分支,并切换到该分支下

git checkout -b dev origin/dev // 线上有分支,本地没有分支的情况下, 创建一个名为dev的本地分支,并关联线上的dev分支

git push origin dev:dev // 本地有分支,push到线上

git push origin :dev // push空分支,相当于删除分支

git push origin --delete dev // 删除远程分支


```









