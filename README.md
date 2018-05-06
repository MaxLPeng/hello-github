这是个使用github的实验


////////////////////////////////////////////   
//           tool 比较  
////////////////////////////////////////////  
█注册  
git config --global diff.tool bc3  
git config --global difftool.bc3.path "C:\Program Files (x86)\Beyond Compare 3/BCompare.exe"  

█调用  
查看当前目录  
git difftool ./  
比较filename文件  
git difftool filename  


////////////////////////////////////////////      
//           初始化  
////////////////////////////////////////////      
1)  web side  
  => Your profile=>Repositories=>New=>Repository name

2) 本地目录下  
$ cd hello-github/  
$ pwd  
/e/WorkStation/GitHub/hello-github  
$ vim helloGit.txt  
$ git init  
$ git add .  
$ git commit -m "init"  
$ git remote add origin https://github.com/MaxLPeng/hello-github.git  
$ git push -u origin master  
 
////////////////////////////////////////////      
//           增、删、改、撤消  
////////////////////////////////////////////      
█添加 文件  
$ vim c.txt  
$ git add c.txt  
$ git commit -m "add new file"  
$ git push

█添加 文件夹（空文件夹无效要有文件）  
$ mkdir b  
$ vim b/bb.txt  
$ git add .  
$ git commit -m "new folder"  
$ git push  

█删除 文件  
$ git rm b.txt  
$ git commit -m "rm b.txt"  
$ git push  

█删除 文件夹  
$ ll a/*  
$ git rm -r a/  
$ $ git commit -m "rm folder"  
$ git push  

█修改 文件  
$ vim hello-git.txt  
$ git add hello-git.txt  
$ git commit -m " edit file"  
$ git push  

█撤消 修改/删除（未commit）  
$ git checkout HEAD hello-git.txt  

█撤消 修改/删除（已经commit，未push）  
$ git log --oneline -3  
	c5f15c0 (HEAD -> master) append line3  
	181fca5 (origin/master) append line 2  
	934e71a  edit file  
$ git reset --hard 181fca5  
$ git pull  

或者
$ git reset --hard HEAD^  
$ git pull  

█回退文件 1（已push）  
$ git log --oneline -4  
	7ced136 (HEAD -> master, origin/master) add point B  
	fd68d2c add point A  
	ddc656f edit line 3  
	38058b0 append new line  
$ git reset fd68d2c helloGit-two.txt  
$ git commit -m "rollback old file "  
$ git checkout helloGit-two.txt  
$ git push origin master  

█回退文件 2（已push）  
$ git revert 7ced136b  
error: could not revert 7ced136... add point B  
hint: after resolving the conflicts, mark the corrected paths  
hint: with 'git add <paths>' or 'git rm <paths>'  
hint: and commit the result with 'git commit'  

$ git revert --abort  
$ git log --oneline -5  
a09f92f (HEAD -> master, origin/master) add new line point c  
a3990b2 rollback old file  
7ced136 add point B  
fd68d2c add point A  
ddc656f edit line 3  

$ git revert a09f92f --no-commit  
$ git revert a3990b2 --no-commit  
$ git revert 7ced136 --no-commit  
$ git revert --continue  
$ git push  

////////////////////////////////////////////      
//           冲突解决  
////////////////////////////////////////////      
█方式1 (pull->修改冲突->commit->push)  
$ cat helloGit-two.txt  
....  
point 11(add by local side)   
$ git add .   
$ git commit -m "local add ponit 11"  
$ git push  
To https://github.com/MaxLPeng/hello-github.git  
 ! [rejected]        master -> master (fetch first)  
error: failed to push some refs to 'https://github.com/MaxLPeng/hello-github.git'  
hint: Updates were rejected because the remote contains work that you do  
......  

$ git pull  
$ cat helloGit-two.txt  
....  
<<<<<<< HEAD  
point 11(add by local side)  
  
=======  
point D (add by web side)  
\>>>>>>> 0cf4bb7afcafb76db3499968514b38aa23610a2e  

$ vim helloGit-two.txt  
....  
point 11(add by local side)  
point D (add by web side)  
  
$ git add .   
$ git commit -m "merge $ add new "  
$ git push  

█方式2   (stash->修改冲突->commit->push->stash apply->commit->push->stash clear)  
编辑新内容  
$ vim helloGit-two.txt  
 要处理bug了  
 将上面修改保存到Git栈中,让工作区和线上一致  
$ git stash  
$ git stash list  
stash@{0}: WIP on master: 88d942e merge $ add new  
在回复后的工作区中修改文件  
$ vim helloGit-two.txt  
$ git add .  
$ git commit -m "fix something , frist"  
$ git push  
修改好后 继续前次新内容  
$ git stash apply  
新内容修改好后提交  
$ vim helloGit-two.txt  
$ git add .  
$ git commit -m "add new line by stash, after fix somethings"  
$ git push 
$ git pull 
$ git stash clear

////////////////////////////////////////////      
//           标签  
////////////////////////////////////////////      
查看 git tag  
标记 git tag -a v0.1.0 -m "release 0.1.0 version"  
删除 git tag -d v0.1.0  
补打 git tag -a v0.1.0 49e0cd22f6bd9510fe65084e023d9c4316b446a6  
发布 git push origin v0.1.0  
切换 git checkout v0.1.0  

////////////////////////////////////////////      
//           分支  
////////////////////////////////////////////      
█创建分支dev  
$ git checkout -b dev  
查看  
$ git branch  
\* dev  
  master  
$ cat helloGit.txt  
...  
line 5 (add on web)  
修改文件  
$ vim helloGit.txt  
$ cat helloGit.txt  
...  
line 5 (add on web)  
line 1111 (add by branch dev)  
  
█提交分支  
$ git add .  
$ git commit -m "by -b dev"   
$ git push origin dev  
  
█完成任务后，准备合并  
█切换到主分支  
$ git checkout master  

█合并  
$ git merge dev  
Updating eb8d9a3..97d6c79  
Fast-forward  
 helloGit.txt | 2 +-  
 1 file changed, 1 insertion(+), 1 deletion(-)  
查看  
$ cat helloGit.txt  
...  
line 5 (add on web)  
line 1111 (add by branch dev)  

█删除分支  
$ git branch -d dev  
$ git branch  
\* master  
$ git push origin master  

查看所有  
$ git branch -a  
\* master 
  remotes/origin/dev  
  remotes/origin/master  

█删除远程分支  
$ git push origin :dev  
To https://github.com/MaxLPeng/hello-github.git  
 - [deleted]         dev  


