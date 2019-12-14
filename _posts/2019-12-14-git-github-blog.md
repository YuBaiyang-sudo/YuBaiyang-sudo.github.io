---
title: git使用笔记
tags: git github
categories: algorithm
---

* TOC
{:toc}

# Git  安装与配置
>安装命令： sudo apt-get install git
>安装完成后： git      （测试是否安装成功）

# 创建一个版本库
>（1）新建一个目录       git_test      在git_test目录下创建一个版本库，命令如下
>> git  init     初始化git仓库   会生成一个	.git   文件 	(版本库目录)	
>> mkdir git_test	创建	git_test	目录

# Git使用
1.在	git_test	目录下创建一个文件	code.txt	
		向目录中添加内容	vi	code.txt         (  :wq  保存修改并退出 vi )

--------
# vi 使用命令
	打开    vi  文件路径（或文件名）（如果文件存在则打开现有文件，如果文件不存在则新建文件，并在终端最下面一行显示打开的是一个新文件）
	写入   键盘输入字母 “i”或“Insert”键进入最常用的插入编辑模式
	保存   
		在插入编辑模式下编辑文件
		按下 “ESC” 键，退出编辑模式，切换到命令模式
		在命令模式下键入"ZZ"或者":wq"保存修改并且退出 vi
		如果只想保存文件，则键入":w"，回车后底行会提示写入操作结果，并保持停留在命令模式
	放弃修改
		放弃所有文件修改：按下 "ESC" 键进入命令模式，键入 ":q!" 回车后放弃修改并退出vi
		放弃所有文件修改，但不退出 vi ，即回退到文件打开后最后一次保存操作的状态，继续进行文件操作：按下 "ESC" 键进入命令模式，键入 ":e!" ，回车后回到命令模式
--------

2.使用如下两条命令可以创建一个版本
		git	add   code.txt
		git	commit 	 -m  ‘版本1’
3.查看版本命令
		git   log
4.版本回退(回到某一个版本)
	HEAD^   回到上一个版本
	HEAD^^  回到上两个版本
	HEAD～n    n为想退回版本的次数
		 git	reset   --hard  HEAD^ 
5.查看之前操作记录
		git	reflog	
		git 	reset  --hard  版本序列号    （撤销暂存）

# 工作区
	电脑中的目录，比如我们的   qit_test   ,就是一个工作区

# 版本库
	工作区有一个隐藏目录，git，这个不是工作区，而是git的版本库
		git 的版本库里存了很多东西，最重要的就是称为 stage 的暂存区，还有git 为我们自动创建的第一个分支master ，以及指向master 的一个指针叫HEAD
	把文件往git 版本库里添加的时候，分为两步：
		第一步用  git  add  把文件添加进去，实际上就是把文件修改添加到暂存区
		第二步用  git  commit  提交更改，实际上就是把暂存区的所有内容提交到当前分支、
	查看当前工作状态
		git  status  

# 管理和修改
	git 管理的文件的修改， 它只会提交暂存区的修改来创建版本
		git  add   
		git  commit  -m

# 撤销修改
	git  checkout  --  code.txt    (工作区撤销修改)
	git  reset  HEAD  code.txt   (添加到暂存区之前撤销修改)


# 小结：
	当改乱工作区某个文件的内容，想直接丢弃工作区的修改时
		git  checkout  --  file
	当改乱工作区某个文件的内容并且添加到了暂存区，想丢弃修改
		第一步：git  reset  HEAD  file
		第二步：git  checkout  --  file
	当改乱工作区某个文件的内容并且添加到了暂存区还到了版本库，想要撤销本次
		版本回退


# 对比文件的不同
	git  diff  HEAD  –  code.txt
		---  (减号)  对应HEAD版本code.txt
		++  (加号)  对应工作区的code.txt
			没有出现+或- 代表两个文件共有的
			如果出现代表哪个里特有的

# 删除文件
	rm  code.txt     （删除也是一种修改，可以撤销修改）
	git  rm  code.txt    (删除改动放到暂存区里，可以撤销改动)
		git  rm  用于删除一个文件，如果一个文件已经被提交到版本库，那么永远不用担心误删，只能回复文件到最新版本，你会丢失最近一次提交后你修改的内容

# 简短形式显示版本号
	git  log 	--  pretty=oneline 


## 分支
	
	1.查看当前有几个分之并且看到在哪个分之下工作
		>git  branch
	
	2.创建一个新的分之
		>git  branch  <name>(分支名)

	3.创建一个新的dev分之并切换dev分之工作（之前的记录都会有）
		>git  checkout  -b  dev

	4.切换分之
		>git  checkout  master(分支名)

	5.合并分之内容		(当前是master分之)
		>git  merge  dev (分支名)(合并到新的分之位置)（快速合并，指针向前移动）
			>合并完成后就可以放心的删除dev分支了，删除后，查看branch，就剩下master分支了
			>通常，合并分之时，如果可整，git  会用 fast  forward  模式(快速合并)，但是有些快速合并不能成而且合并时没有冲突，这个时候会合并之后并做一次新的提交。但是这种模式下，删除分之后，会丢掉分之信息。
		>禁止快速合并模式
				git  merge  --  no - ff   -m  ‘禁用fast-forward’  dev(分支名)
			会做一次新一次提交，两个指针会向前移（合并dev分）

	6.显示未与当前分之合并的分之
		git  branch  - - no – merged

	7.分之在合并时候可能会起冲突
		两个分支都有新的提交记录并且修改的是同一个文件，手动解决，在add提交

	8.删除分之
		git  branch  -d  dev(分支名 )			


突然处理bug
	把工作现场保存起来，等以后回复现场后继续工作
		git  stash       (工作区就变干净了)
	切换到要修复bug的分之上
	创建临时修复分之		git  checkout  -b  bug – 001
	在新分支处理错误
		vi 进入文档修改内容
		修改之后做一次提交  git add  文件名
							   git  commit  -m  ‘修复bug-001’
	回到原来分支	git  checkout  master(原来分支名)
	（合并分之	git  merge  bug-001	(快递合并，看不出来合并记录)）
	禁止快速合并模式合并分之	git  merge   - - no – ff   -m   ‘修复bug-001’   bug-001 (文件名)
	删除临时的bug-001分之	git  branch  -d  bug-001
	找回刚才保存的工做现场
		列出保存工作现场	git  stash  list
		恢复保存的工现场	git  stash  pop



生成SSH秘钥
	ssh-keygen  -t  rsa  -C  ‘923772781@qq.com’
	三次回车
	cd  .ssh/   ( ls  里面会有三个文件  id_rsa   id_rsa.pub   known_hosts)
	cat  id_rsa.pub  (就能看到公钥内容)
	把内容复制放到github SSH和GPG秘钥被里

克隆项目
	把仓库里SSH地址复制
	git  clone  (SSH地址)

推送分支（将自己代码推到远程上）
	git  push  origin(git@github.com:YuBaiyang-sudo/(yubai.git))  master

将本地发分之跟踪远程的分支
	git  branch  – set – upstream – to=origin/分支名称  分支名称000

从远程分支上拉取代码
	git  pull  origin  分之名称
