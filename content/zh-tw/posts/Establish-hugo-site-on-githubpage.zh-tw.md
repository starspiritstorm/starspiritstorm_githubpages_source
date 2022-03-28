---

title: "在 Github Pages 上建立 Hugo"
author: starspiritstorm
type: posts
date: 2022-03-18T12:00:00+08:00
draft: false
categories: ["hugo"]
tags: ["hugo", "theme-meme", "blog", "github", "github pages", "github action"]
keywords: ["establish hugo in githubpage"]

---


在 Github Pages 上建立 Hugo。

<!--more-->


## 建立 Hugo Site

### 本地端


先到你喜歡的資料夾裡面，開啟 git bash


![fig01](/images/establish-hugo-site-on-githubpage/fig01.start_git_bash_here.png "Fig1. start git bash here")


hugo 的產生 hugo site 相關內容的指令 

	hugo new site myblog

進入剛剛創好的

	cd myblog/


![fig02](/images/establish-hugo-site-on-githubpage/fig02.hugo_new_site.png "Fig2. hugo new site")
![fig03](/images/establish-hugo-site-on-githubpage/fig03.cd_folder.png "Fig3. cd folder")


### 安裝 Hugo Theme - meme


可以到 [hugoranked](https://hugoranked.com/) 看星星數多的，

或是直接到 [hugo 的官方網站](https://themes.gohugo.io/) 的theme挑選喜歡的主題，

這邊我挑的是 theme/meme，

不同的 theme 請看各個 theme 的教學引導。



利用 git 取得 theme meme

	git init
	git submodule add --depth 1 https://github.com/reuixiy/hugo-theme-meme.git themes/meme

套用 hugo theme meme 的config.toml

	rm config.toml && cp themes/meme/config-examples/en/config.toml config.toml


![fig05](/images/establish-hugo-site-on-githubpage/fig05.git_command.png "Fig5. git command")


修改 config.toml 的 baseurl = "https://[your_github_username].github.io/"

例：https://starspiritstorm.github.io/


![fig05-1](/images/establish-hugo-site-on-githubpage/fig05-1.edit_config.png "Fig5-1. edit config")
![fig05-2](/images/establish-hugo-site-on-githubpage/fig05-2.rename_url.png "Fig5-2. rename url")
![fig05-3](/images/establish-hugo-site-on-githubpage/fig05-3.rename_url_yours.png "Fig5-3. rename url yours")



產生簡單的 post&about

	hugo new "posts/hello-world.md"
	hugo new "about/_index.md"


![fig06](/images/establish-hugo-site-on-githubpage/fig06.generate_posts.png "Fig6. generate posts")



這邊有一點要注意的是，hugo new 產生的文章預設 draft 都是 true，用 notepad++ 開啟，

並把 draft 設定改成 false 才會真正的在 Github Pages 上顯示出來。

路徑在分別在

myblog\content\posts

myblog\content\about


![fig06-1](/images/establish-hugo-site-on-githubpage/fig06-1.file_in_posts.png "Fig6-1. file in posts")
![fig06-2](/images/establish-hugo-site-on-githubpage/fig06-2.file_in_about.png "Fig6-2. file in about")


![fig07-1](/images/establish-hugo-site-on-githubpage/fig07-1.about_draft_false.png "Fig7-1. about draft false")
![fig07-2](/images/establish-hugo-site-on-githubpage/fig07-2.post_draft_false.png "Fig7-2. post draft false")


### 本地端檢驗 Hugo site


開啟

	hugo server -D


![fig08](/images/establish-hugo-site-on-githubpage/fig08.hugo_server.png "Fig8. hugo server")


到瀏覽器 URL 輸入

	http://localhost:1313/

檢查本地端是否能成功看到剛剛建好的網頁


![fig09](/images/establish-hugo-site-on-githubpage/fig09.hugo_blog_in_local.png "Fig9. hugo blog in local")



## 部屬至 Github Pages

### 先產生 Github Pages 的 repository


先去 github 產生你的 Github Pages 需要用到的 repository。

repository 的名字必須是 "[你的github帳號].github.io"。


![fig10](/images/establish-hugo-site-on-githubpage/fig10.create_yourname.github.io_step1.png "Fig10. step1 yourname.github.io")
![fig11](/images/establish-hugo-site-on-githubpage/fig11.create_yourname.github.io_step2.png "Fig11. step2 yourname.github.io")
![fig12](/images/establish-hugo-site-on-githubpage/fig12.create_yourname.github.io_step3.png "Fig12. step3 yourname.github.io")



### 在 myblog 下，產生 hugo 的 public 資料夾


再回到剛剛創好的 myblog 路徑下，直接執行

	hugo


![fig14](/images/establish-hugo-site-on-githubpage/fig14.execute_hugo.png "Fig14. execute hugo")



會看到 public 資料夾產生，然後進入 public 資料夾

	cd public/


public 資料夾是 hugo for Github Pages 靜態頁面用的，也就是

https://github.com/[你的帳號]/[你的帳號].github.io

這個 repository 要用的。


### 在 public 資料夾，開啟 git，要上 github 用的


接下來要在 public 資料夾裡面開 git 才能推到 github 上。


	git init
	git remote add origin https://github.com/[你的帳號]/[你的帳號].github.io

例：git remote add origin https://github.com/starspiritstorm/starspiritstorm.github.io

	git add .
	git commit -m "Initial commit"
	git push --set-upstream origin master


![fig15](/images/establish-hugo-site-on-githubpage/fig15.init_git_in_public1.png "Fig15. init git in public")
![fig16](/images/establish-hugo-site-on-githubpage/fig16.init_git_in_public2.png "Fig16. init git in public2")



### 到 github 網頁設定 Source


推完之後還需要去設定。

Settings > Options > Github Pages > Source to “master branch”


![fig18](/images/establish-hugo-site-on-githubpage/fig18.github_setting_source1.png "Fig18. github setting source1")
![fig19](/images/establish-hugo-site-on-githubpage/fig19.github_setting_source2.png "Fig19. github setting source2")
![fig20](/images/establish-hugo-site-on-githubpage/fig20.github_setting_source3.png "Fig20. github setting source3")


推完之後就可以到 你的 Github Pages (https://[你的帳號].github.io/) 看看結果了。


![fig22](/images/establish-hugo-site-on-githubpage/fig22.github_pages_result.png "Fig22. github pages result")



## Github Pages hugo source code 

### 產生 Github Pages source code 的 repository


這邊要注意的是，所有文章的 source code 還是存放在你的本地端，
剛剛的 Github Pages 實際上只有上傳了 public 資料夾內的東西到你的 Github Pages 的 repository。


所以還需要再開另外一個 github repository，來儲存你的 source code，
步驟跟剛剛開 Github Pages 的方法一樣，名稱可以自己取一個喜歡的，這邊我是直接這樣取。

	https://github.com/[你的帳號]/[你的帳號]_githubpages_source

例：https://github.com/starspiritstorm/starspiritstorm_githubpages_source


![fig24](/images/establish-hugo-site-on-githubpage/fig24.create_source_code_repository.png "Fig24. create source code repository")


### 回 myblog 資料夾，開啟 git，要上 github_source_code 用的


創好之後再回到 myblog 資料夾重新開一個 git bash。


![fig25](/images/establish-hugo-site-on-githubpage/fig25.open_git_bash_in_myblog.png "Fig25. open git bash in myblog")


然後產生一個 gitignore 把 public 資料夾排除，因為 public 是 hugo build 出來的產物

	echo "public/" >> .gitignore


![fig26](/images/establish-hugo-site-on-githubpage/fig26.git_ignore_public.png "Fig26. git ignore public")


然後做一樣的事情把你的 source code 推到新的 repository。

	git init
	git remote add origin https://github.com/[你的帳號]/[你的帳號]_githubpages_source

例：git remote add origin https://github.com/starspiritstorm/starspiritstorm_githubpages_source
	
	git add .
	git commit -m "Add hugo source code"
	git push --set-upstream origin master


![fig27](/images/establish-hugo-site-on-githubpage/fig27.init_git_in_myblog.png "Fig27. init git in myblog")



## 自動化部屬 (GitHub Action)，只要推 source code，就會幫你更新 Github Pages

### Submodule 要產一個 public 出來


最後一步驟，藉由 GitHub Action 來做自動化部署。


為了後面要能自動化部署，需要先產生 submodule public 預備著[^1]。

	git submodule add -f --depth 1 https://github.com/[你的帳號]/[你的帳號].github.io.git public

例：git submodule add -f --depth 1 https://github.com/starspiritstorm/starspiritstorm.github.io.git public


產生完之後記得 commit \& push。


![fig28](/images/establish-hugo-site-on-githubpage/fig28.submodule_commit.png "Fig28. submodule commit")



### 產生 github Personal Access Token


先到 [這個頁面](https://github.com/settings/tokens/new) ，產生 Personal Access Token (PAT)。

名稱可以自取，過期時間先選 No expired，因為也只會給 workflow 用，如果之後有疑慮可以重新產生 token。

勾選 workflow

![fig30](/images/establish-hugo-site-on-githubpage/fig30.generate_github_personal_access_token.png "Fig30. generate github personal access token")


拉到下面按產生

![fig31](/images/establish-hugo-site-on-githubpage/fig31.click_generate.png "Fig31. click generate")


產生完以後先複製token的值

![fig32](/images/establish-hugo-site-on-githubpage/fig32.PAT_generated.png "Fig32. PAT generated")


到存放你 source code 的 repository 去設定剛剛產生好的 token

Settings > Secret > New repository secret

![fig33](/images/establish-hugo-site-on-githubpage/fig33.use_token.png "Fig33. use token")


取名 HUGO_DEPLOY_TOKEN，並貼上剛剛複製的token的值

![fig34](/images/establish-hugo-site-on-githubpage/fig34.generate_new_secret.png "Fig34. generate new secret")


### 設定 GitHub Action 的 workflow


到 Github Action 頁面，點選 set up a workflow your self

![fig35](/images/establish-hugo-site-on-githubpage/fig35.set_up_a_workflow.png "Fig35. set up a workflow")


在左邊 Edit New file 裡面把下面的 script 貼上後，
記得把 external_repository 修改成你的 Github Pages 的 repository。

按下 start commit 後等他跑完。

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
										  #[2]：這邊名稱要填入剛剛在 secret 產生的 token 的名稱，此行記得刪除
			  personal_token: ${{ secrets.HUGO_DEPLOY_TOKEN }}
								   #[3]：這邊要輸入的是你的 Github Pages 的 repo，此行記得刪除
			  external_repository: starspiritstorm/starspiritstorm.github.io
			  publish_branch: master
			  publish_dir: ./public
			  commit_message: ${{ github.event.head_commit.message }}
			  # https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
			  TZ: Asia/Taipei



---

> **_[2] 難點2:_**
>
> 當初不知道那個「secrets.後面這串」是要填剛剛在 secret 產生的 token 的名稱。
> 
> 就在 Action 的 Deploy 裡面遇到錯誤。
>
> Error: Action failed with "not found deploy key or tokens"
> 
> ![fig002](/images/establish-hugo-site-on-githubpage/fig002.not_found_deploy_key_or_tokens.png "Fig002. not found deploy key or tokens")
> 


> **_[3] 難點3:_**
>
> 這邊原本我複製 Github Pages 的時候多複製到了.git，變成 starspiritstorm/starspiritstorm.github.io.git。
> 
> 結果就在 Action 的 Deploy 裡面遇到錯誤。
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


最後看到 source code 的 Action 畫面成功地顯示 build 綠勾勾，就代表你成功的建立好流程了。


![fig40](/images/establish-hugo-site-on-githubpage/fig40.workflow_finished.png "Fig40. workflow finished")

![fig41](/images/establish-hugo-site-on-githubpage/fig41.build_sucess.png "Fig41. build sucess")



## 自動化部屬測試



之後只需要在 source code 的 repository 新增 post 就會自動更新到 Github Pages 上了。

一樣在 myblog 資料夾開 Git Bash，然後 hugo new 一個 post。


![fig44](/images/establish-hugo-site-on-githubpage/fig44.new_post.png "Fig44. new post")


然後 commit 之後 push 到 origin，再去 Action 看，可以看到成功的內容。


![fig45](/images/establish-hugo-site-on-githubpage/fig45.auto_deploy.png "Fig45. auto deploy")


然後可以再去你的 Github Pages 看看剛剛新增的 post。


![fig46](/images/establish-hugo-site-on-githubpage/fig46.deployed_post1.png "Fig46. deployed_post1")

![fig47](/images/establish-hugo-site-on-githubpage/fig47.deployed_post2.png "Fig47. deployed_post2")



最後一個要注意的就是當初 config.toml 是直接複製範本過來用的，所以裡面有一些資訊要記得做修正。
請參考[下篇文章](https://starspiritstorm.github.io/en/posts/setting-config.toml/)。


## Reference

1. [theme meme 作者 reuixiy 的安裝教學](https://github.com/reuixiy/hugo-theme-meme)


2. [theme meme 的作者寫的「使用 GitHub Actions 部署 Hugo 博客到 GitHub Pages」](https://io-oi.me/tech/deploy-hugo-to-github-pages-via-github-actions/)


3. Github Action 的參考，不能完全照抄，會失敗...，組合起來嘗試才成功的

[Create and host a blog with Hugo and GitHub Pages in less than 30 minutes](https://www.mytechramblings.com/posts/create-a-website-with-hugo-and-gh/)

[如何將Hugo部落格部署到Github上?](https://yurepo.tw/2021/03/%E5%A6%82%E4%BD%95%E5%B0%87hugo%E9%83%A8%E8%90%BD%E6%A0%BC%E9%83%A8%E7%BD%B2%E5%88%B0github%E4%B8%8A/)



---

[^1]: 難點1：Github Action 需要 submodule 裡面有 public 才能正常作業...不然會在 Github Action 時運作時，在 Run actions/checkout@v2 遇到 Error: fatal: No url found for submodule path 'public' in .gitmodules。
![fig001](/images/establish-hugo-site-on-githubpage/fig001.submodule_error.png "Fig001. submodule error")



