---

title: "Establish Hugo Site on GithubPage"
author: starspiritstorm
type: posts
date: 2022-03-18T12:00:00+08:00
draft: false
categories: ["hugo"]
tags: ["hugo", "theme-meme", "blog", "github", "github pages", "github action"]
keywords: ["establish hugo in githubpage"]

---


Establish Hugo Site on GithubPage.

<!--more-->


## Establish Hugo Site

### Local


Finding your favorite folder, start git bash.


![fig01](/images/establish-hugo-site-on-githubpage/fig01.start_git_bash_here.png "Fig1. start git bash here")


Generate hugo site command

	hugo new site myblog

Change folder path

	cd myblog/


![fig02](/images/establish-hugo-site-on-githubpage/fig02.hugo_new_site.png "Fig2. hugo new site")
![fig03](/images/establish-hugo-site-on-githubpage/fig03.cd_folder.png "Fig3. cd folder")


### Install Hugo Theme - meme


You can go to [hugoranked](https://hugoranked.com/) find with more stars.

Or go to hugo [official website](https://themes.gohugo.io/), check your favorite theme.

Here I choose theme/meme.

If your are choosing different theme, please check their installment instruction.



Using git to get theme meme

	git init
	git submodule add --depth 1 https://github.com/reuixiy/hugo-theme-meme.git themes/meme

Use hugo theme meme's config.toml

	rm config.toml && cp themes/meme/config-examples/en/config.toml config.toml


![fig05](/images/establish-hugo-site-on-githubpage/fig05.git_command.png "Fig5. git command")


Modify config.toml baseurl = "https://[your_github_username].github.io/"

Example: https://starspiritstorm.github.io/


![fig05-1](/images/establish-hugo-site-on-githubpage/fig05-1.edit_config.png "Fig5-1. edit config")
![fig05-2](/images/establish-hugo-site-on-githubpage/fig05-2.rename_url.png "Fig5-2. rename url")
![fig05-3](/images/establish-hugo-site-on-githubpage/fig05-3.rename_url_yours.png "Fig5-3. rename url yours")



Generate simple post&about

	hugo new "posts/hello-world.md"
	hugo new "about/_index.md"


![fig06](/images/establish-hugo-site-on-githubpage/fig06.generate_posts.png "Fig6. generate posts")


One thing you need to take care of is, the draft setting of posts generated by hugo new are all default true.

Open up with notepad++, and change it to false, then it will appeared on Github Pages.


The posts are separated here

myblog\content\posts

myblog\content\about


![fig06-1](/images/establish-hugo-site-on-githubpage/fig06-1.file_in_posts.png "Fig6-1. file in posts")
![fig06-2](/images/establish-hugo-site-on-githubpage/fig06-2.file_in_about.png "Fig6-2. file in about")


![fig07-1](/images/establish-hugo-site-on-githubpage/fig07-1.about_draft_false.png "Fig7-1. about draft false")
![fig07-2](/images/establish-hugo-site-on-githubpage/fig07-2.post_draft_false.png "Fig7-2. post draft false")


### Check Hugo site in local


Turn on the site.

	hugo server -D


![fig08](/images/establish-hugo-site-on-githubpage/fig08.hugo_server.png "Fig8. hugo server")


Typing this url on browser

	http://localhost:1313/

To see if you can see the posts you just generated or not.


![fig09](/images/establish-hugo-site-on-githubpage/fig09.hugo_blog_in_local.png "Fig9. hugo blog in local")



## Deploy to Github Pages

### Create Github Pages's repository first


Go to your github to generate your Github Pages repository。

The name of Github Pages repository must be "[your_github_account].github.io".


![fig10](/images/establish-hugo-site-on-githubpage/fig10.create_yourname.github.io_step1.png "Fig10. step1 yourname.github.io")
![fig11](/images/establish-hugo-site-on-githubpage/fig11.create_yourname.github.io_step2.png "Fig11. step2 yourname.github.io")
![fig12](/images/establish-hugo-site-on-githubpage/fig12.create_yourname.github.io_step3.png "Fig12. step3 yourname.github.io")



### Generate hugo public folder in myblog


Back to the myblog, execute

	hugo


![fig14](/images/establish-hugo-site-on-githubpage/fig14.execute_hugo.png "Fig14. execute hugo")



Then you will see the public folder, change folder to public

	cd public/


The public folder is for hugo Github Pages, it's for this repository

https://github.com/[your_github_account]/[your_github_account].github.io



### Initial git in public folder, for pushing to the github


You need to initialize git in public folder to push it on github.


	git init
	git remote add origin https://github.com/[your_github_account]/[your_github_account].github.io

Example: git remote add origin https://github.com/starspiritstorm/starspiritstorm.github.io

	git add .
	git commit -m "Initial commit"
	git push --set-upstream origin master


![fig15](/images/establish-hugo-site-on-githubpage/fig15.init_git_in_public1.png "Fig15. init git in public")
![fig16](/images/establish-hugo-site-on-githubpage/fig16.init_git_in_public2.png "Fig16. init git in public2")



### Setting source in github webpage


After pushing the code into GitHub you still need to modify the Repository settings.

Change the setting found in: Settings > Options > Github Pages > Source to “master branch”


![fig18](/images/establish-hugo-site-on-githubpage/fig18.github_setting_source1.png "Fig18. github setting source1")
![fig19](/images/establish-hugo-site-on-githubpage/fig19.github_setting_source2.png "Fig19. github setting source2")
![fig20](/images/establish-hugo-site-on-githubpage/fig20.github_setting_source3.png "Fig20. github setting source3")


You can check the result after pushing it to your Github Pages (https://[your_github_account].github.io/).


![fig22](/images/establish-hugo-site-on-githubpage/fig22.github_pages_result.png "Fig22. github pages result")



## Github Pages hugo source code 

### Create Github Pages source code's repository


Note that all articles source code is stored in your local side.
The previous pushed to your Github Pages is just your public folder.


So you need to open up another repository to store your source code.
The steps are same with previous Github Pages repository, you can name your favorite naming for your source code repository.


	https://github.com/[your_github_account]/[your_github_account]_githubpages_source

Example: https://github.com/starspiritstorm/starspiritstorm_githubpages_source


![fig24](/images/establish-hugo-site-on-githubpage/fig24.create_source_code_repository.png "Fig24. create source code repository")


### Back to myblog folder, open git bash for github_source_code


After finished repository, open up git bash in myblog.


![fig25](/images/establish-hugo-site-on-githubpage/fig25.open_git_bash_in_myblog.png "Fig25. open git bash in myblog")


Then generate a gitignore to exclude public folder, we don't need it because it is result from hugo build.

	echo "public/" >> .gitignore


![fig26](/images/establish-hugo-site-on-githubpage/fig26.git_ignore_public.png "Fig26. git ignore public")


And do the same steps to push your myblog's source code to your new repository.

	git init
	git remote add origin https://github.com/[your_github_account]/[your_github_account]_githubpages_source

Example: git remote add origin https://github.com/starspiritstorm/starspiritstorm_githubpages_source
	
	git add .
	git commit -m "Add hugo source code"
	git push --set-upstream origin master


![fig27](/images/establish-hugo-site-on-githubpage/fig27.init_git_in_myblog.png "Fig27. init git in myblog")


## Automatic deployment (GitHub Action)

### Submodule needs a public folder


Last step, use GitHub Action to do automatic deployment.
When you push your source code to your source code repo, GitHub Action will generate the result to your GithubPage.


For the automatic deployment, you need to generate submodule public first[^1].

	git submodule add -f --depth 1 https://github.com/[your_github_account]/[your_github_account].github.io.git public

Example: git submodule add -f --depth 1 https://github.com/starspiritstorm/starspiritstorm.github.io.git public


And remember commit \& push.


![fig28](/images/establish-hugo-site-on-githubpage/fig28.submodule_commit.png "Fig28. submodule commit")



### Create github Personal Access Token


Go to [here](https://github.com/settings/tokens/new) to create Personal Access Token (PAT).

Naming no restricted, expired time I choose "No expired", you can regenerate token if you want.

Check workflow

![fig30](/images/establish-hugo-site-on-githubpage/fig30.generate_github_personal_access_token.png "Fig30. generate github personal access token")


Pull down and click Generate token.

![fig31](/images/establish-hugo-site-on-githubpage/fig31.click_generate.png "Fig31. click generate")


And copy the token value.

![fig32](/images/establish-hugo-site-on-githubpage/fig32.PAT_generated.png "Fig32. PAT generated")


Using the generated token value to set up your repository secret.

Settings > Secret > New repository secret

![fig33](/images/establish-hugo-site-on-githubpage/fig33.use_token.png "Fig33. use token")


Name HUGO_DEPLOY_TOKEN, and paste the copied token value.

![fig34](/images/establish-hugo-site-on-githubpage/fig34.generate_new_secret.png "Fig34. generate new secret")


### Setting GitHub Action workflow


Go to Github Action webpage, click "set up a workflow your self"

![fig35](/images/establish-hugo-site-on-githubpage/fig35.set_up_a_workflow.png "Fig35. set up a workflow")


Paste script in the "Edit new file", remember changing external_repository to your Github Pages repository.


Click "start commit", and wait.

![fig36](/images/establish-hugo-site-on-githubpage/fig36.use_script.png "Fig35. use script")


	name: hugo CI

	on:
	  push:
		branches: [ master ]

	jobs:
	  build:
		runs-on: ubuntu-latest

		steps:
		  - uses: actions/checkout@v2
			with:
			  submodules: true 
			  fetch-depth: 1

		  - name: Setup Hugo
			uses: peaceiris/actions-hugo@v2
			with:
			  hugo-version: 'latest'
			  extended: true 

		  - name: Build
			run: hugo

		  - name: Deploy
			uses: peaceiris/actions-gh-pages@v3
			with:
										  #[2]: Here you need fill the secret token name, remember delete this line.
			  personal_token: ${{ secrets.HUGO_DEPLOY_TOKEN }}
								   #[3]: Here you need to type your Github Pages repository, remember delete this line.
			  external_repository: starspiritstorm/starspiritstorm.github.io
			  publish_branch: master
			  publish_dir: ./public
			  commit_message: ${{ github.event.head_commit.message }}
			  # https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
			  TZ: Asia/Taipei



---

> **_[2] Difficult 2:_**
>
> I didn't know that 「secrets.[token_name]」 needs the name of secret token.
> 
> So I encountered the error in Action Deploy.
>
> Error: Action failed with "not found deploy key or tokens"
> 
> ![fig002](/images/establish-hugo-site-on-githubpage/fig002.not_found_deploy_key_or_tokens.png "Fig002. not found deploy key or tokens")
> 


> **_[3] Difficult 3:_**
>
> I was miscopied my Github Pages url "starspiritstorm/starspiritstorm.github.io.git", the .git is not needed.
> 
> So I encountered another error in Action Deploy.
> 
> remote: Repository not found.
>
> fatal: repository 'https://github.com/starspiritstorm/starspiritstorm.github.io.git.git/' not found
>
> Error: Action failed with "The process '/usr/bin/git' failed with exit code 128"
>
> ![fig003](/images/establish-hugo-site-on-githubpage/fig003.repository_not_found.png "Fig003. repository not found")
> 

---


Finally, when you see that Action page with build green check, the workflow are done!


![fig40](/images/establish-hugo-site-on-githubpage/fig40.workflow_finished.png "Fig40. workflow finished")

![fig41](/images/establish-hugo-site-on-githubpage/fig41.build_sucess.png "Fig41. build sucess")



## Automatic deployment test



After finished Github Action, you just need to new a post in your source code repository, 
the Github Action will do the rest work for you.

Again, start Git Bash in myblog folder, and hugo new post.


![fig44](/images/establish-hugo-site-on-githubpage/fig44.new_post.png "Fig44. new post")


Commit \& push, go to your Action, you will see the sucess result.


![fig45](/images/establish-hugo-site-on-githubpage/fig45.auto_deploy.png "Fig45. auto deploy")


Then you can check the post you just made in your Github Pages.


![fig46](/images/establish-hugo-site-on-githubpage/fig46.deployed_post1.png "Fig46. deployed_post1")

![fig47](/images/establish-hugo-site-on-githubpage/fig47.deployed_post2.png "Fig47. deployed_post2")



Last important thing is the config.toml, there are some place need to modify.
Check [next post](https://starspiritstorm.github.io/en/posts/setting-config.toml/).



## Reference

1. [Installment guide from author of theme meme "reuixiy"](https://github.com/reuixiy/hugo-theme-meme)


2. [「使用 GitHub Actions 部署 Hugo 博客到 GitHub Pages」](https://io-oi.me/tech/deploy-hugo-to-github-pages-via-github-actions/)


3. Reference of Github Action, cannot just copy and pastes..., I combined these two to build up my Github Action.

[Create and host a blog with Hugo and GitHub Pages in less than 30 minutes](https://www.mytechramblings.com/posts/create-a-website-with-hugo-and-gh/)

[如何將Hugo部落格部署到Github上?](https://yurepo.tw/2021/03/%E5%A6%82%E4%BD%95%E5%B0%87hugo%E9%83%A8%E8%90%BD%E6%A0%BC%E9%83%A8%E7%BD%B2%E5%88%B0github%E4%B8%8A/)



---

[^1]: Difficult 1：Github Action submodule needs public folder to work, otherwise it will fail in "Run actions/checkout@v2" Error: fatal: No url found for submodule path 'public' in .gitmodules.
![fig001](/images/establish-hugo-site-on-githubpage/fig001.submodule_error.png "Fig001. submodule error")


