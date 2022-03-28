---

title: "Setting config.toml"
author: starspiritstorm
type: posts
date: 2022-03-28T12:00:00+08:00
draft: false
categories: ["hugo"]
tags: ["hugo", "theme-meme", "config.toml"]
keywords: ["setting config.toml"]

---


Config.toml 設定教學。


<!--more-->


## Config.toml setting

### Modified date

找到 # Modified date

	[frontmatter]
		lastmod = ["lastmod", ":git", ":fileModTime", ":default"]


![fig01](/images/setting_config.toml/fig01.modified_date1.png "Fig1. modified date1")


修改成

	[frontmatter]
		date = ['date', 'publishDate', 'lastmod']
		expiryDate = ['expiryDate']
		lastmod = ["lastmod", ":fileModTime", ":git", ":default"]
		publishDate = ['publishDate', 'date']


![fig02](/images/setting_config.toml/fig02.modified_date2.png "Fig2. modified date2")


原本給的設定順序是 ["lastmod", ":git", ":fileModTime", ":default"]，

這邊調整成 ["lastmod", ":fileModTime", ":git", ":default"]。



### Author’s information

找到 # Author’s information

![fig03](/images/setting_config.toml/fig03.author’s_information.png "Fig3. author’s information")


調整成你自己的內容



### Site Info

找到 # Site Info

![fig04](/images/setting_config.toml/fig04.site_info.png "Fig4. site info")


調整成你自己的內容



### Post Share

找到 # Post Share

![fig05](/images/setting_config.toml/fig05.post_share.png "Fig5. site info")


調整成想要 share 的連結



### Footer icon url changing

最後就是 footer 的圖要換連結

![fig06](/images/setting_config.toml/fig06.footer_icon1.png "Fig6. footer icon1")
![fig07](/images/setting_config.toml/fig07.footer_icon2.png "Fig7. footer icon2")


找到 theme/meme/data 裡面的 Socials.toml

![fig08](/images/setting_config.toml/fig08.footer_icon3.png "Fig8. footer icon3")
![fig09](/images/setting_config.toml/fig09.footer_icon4.png "Fig9. footer icon4")


複製一份到你的 myblog/data

![fig10](/images/setting_config.toml/fig10.footer_icon5.png "Fig10. footer icon5")


並修改連結改為你自己的相關的連結

![fig11](/images/setting_config.toml/fig11.footer_icon6.png "Fig11. footer icon6")



## Reference

[Hugo setting configuration](https://gohugo.io/getting-started/configuration/)



