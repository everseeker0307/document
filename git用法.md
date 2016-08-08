>  1. 	ssh-keygen -t rsa -C "everseeker0307@gmail.com"
>  
>  2. 	cat ~/./id_rsa.pub, 复制到Github上SSh keys.
>  
>  3. 	ssh -T git@github.com验证是否成功，如果出现Hi everseeker0307! You've successfully authenticated, but GitHub does not provide shell access.说明成功了.
>  
>  4. 	设置用户名和email.
>  			git config --global user.name "everseeker0307"
>  			git config --global user.email "everseeker0307@gmail.com"
>  		
>  5.	进入到要上传的仓库, git init; 之后添加远程地址, git remote add origin git@github.com:everseeker0307/spring.git
>  
>  6.	查看状态git status.  
>  		如果是第一次push先git pull origin master.
>  
>  			everseekerdeMacBook-Pro:spring everseeker$ git status  
			On branch master
			Initial commit
			Untracked files:
			  (use "git add <file>..." to include in what will be committed)
				.idea/
				cglib/
				jersey/
				pom.xml
				spring.iml
>	
>	7.	git add.
>	
>			git add ./\*
>			git add .idea/
>			
>	8.	git commit -m "first commit"
>	
>	9.	上传到github.
>	
>			git push origin master