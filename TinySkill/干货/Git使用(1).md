## Git 简单使用 ##

#### Git global setup ####

	git config --global user.name "wwh"
	git config --global user.email "wg-dd@qq.com"

#### Create a new repository ####

	git clone git@192.168.2.242:Android/appsj.git
	cd appsj
	touch README.md
	git add README.md
	git commit -m "add README"
	git push -u origin master

#### Existing folder or Git repository ####

	cd existing_folder
	git init
	git remote add origin git@192.168.2.242:Android/appsj.git
	git add . //git add *
	git commit //git commit -u "your decription"
	git push -u origin master