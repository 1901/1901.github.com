---
layout: post
title: Git常用基本操作
---

{% highlight shell %}

# gitconfig配置。参数可选（auto, ture, false）
$ git config --global color.status auto
$ git config --global color.diff auto
$ git config --global color.branch auto
$ git config --global color.interactive auto
$ git config --global color.ui auto


$ git clone git://github.com/1901/test.git	# 复制一个已存在的Git仓库到当前目录
$ git init							# 在当前目录创建一个Git仓库（一般在工作目录使用，用于创建本地版本库）
$ git init -bare					# 在当前目录创建一个Git仓库（只包含版本历史，不包含其他文件，一般用于创建远程分享版本库）


$ git status						# 查看当前工作目录与缓存的状态
$ git status -s						# 同上（简洁模式查看）
$ git add hello.c					# 添加一个hello.c到缓存区				
$ git rm hello.c					# 将hello.c从缓存区移除（默认情况文件也会从工作目录移除）
$ git mv hello.c hi.c				# 将缓存区的hello.c更改为hi.c（工作目录的文件名也会相应变化）
$ git reset HEAD -- hello.c			# 取消已缓存的内容
$ git commit -m "commit message"	# 提交至本地库
$ git commit -am "commit message"	# 将变动提交至缓存区并提交到本地库
$ git diff							# 查看未缓存的改动
$ git diff --cached					# 查看已缓存的改动
$ git diff HEAD						# 查看未缓存和已缓存的所有改动
$ git diff --stat					# 显示改动摘要信息而非整个diff


$ git branch						# 列出当前可用分支，分支名前带*的为当前分支
$ git branch test_branch			# 创建一个名为test_branch的分支
$ git checkout test_branch			# 切换至名为test_branch的分支
$ git checkout -b test_branch		# 创建test_branch并切换到这个分支（以上两个步骤的简洁操作）
$ git branch -d test_branch			# 删除名为test_branch的分支
$ git merge test_branch				# 合并test_branch到当前分支


$ git remote						# 列出远程库的别名
$ git remote add origin ~/my/Git/	# 添加本地库到远程库，或者说关联本地库与远程库
$ git remote rm origin				# 删除名为origin的远程库
$ git push origin master			# 提交本地库的分支(master)变动到远程库(origin)
$ git pull origin master			# 把远程库(origin)的内容更新到本地当库并合并到当前分支(master)




{% endhighlight %}
