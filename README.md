这是个使用github的实验

█初始化  

1) https://github.com 
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



