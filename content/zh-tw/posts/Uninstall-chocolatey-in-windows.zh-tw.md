---

title: "在 Windows 環境下移除 Chocolatey"
author: starspiritstorm
type: posts
date: 2021-12-21T16:20:52+08:00
draft: false
categories: ["chocolatey"]
tags: ["windows", "chocolatey"]
keywords: ["install chocolatey in windows"]

---


參考 [Chocolatey 官方網站 How to uninstall](https://docs.chocolatey.org/en-us/choco/uninstallation)


<!--more-->


## 移除 Chocolatey

### 移除步驟 1. 砍掉 Chocolatey 的資料夾


把安裝路徑的資料夾砍掉，通常在 C:\ProgramData\chocolatey，不確定在哪的話可以用這行指令 

	$env:ChocolateyInstall 


![fig1](/images/uninstall_chocolately/fig1.check_chocolately_location.png "Fig1. 檢查 Chocolatey 的位置")

![fig2](/images/uninstall_chocolately/fig2.delete_chocolately_folder.png "Fig2. 砍掉 Chocolatey 的資料夾")


### 移除步驟 2. 環境變數移除 Chocolatey


砍掉資料夾以後，還有環境變數的設定要砍


![fig3](/images/uninstall_chocolately/fig3.remove_chocolately_from_environment1.png "Fig3. 移除 Environment 內的 Chocolatey 步驟1")

![fig4](/images/uninstall_chocolately/fig4.remove_chocolately_from_environment2.png "Fig4. 移除 Environment 內的 Chocolatey 步驟2")

![fig5](/images/uninstall_chocolately/fig5.remove_chocolately_from_environment3.png "Fig5. 移除 Environment 內的 Chocolatey 步驟3")

![fig6](/images/uninstall_chocolately/fig6.remove_chocolately_from_environment4.png "Fig6. 移除 Environment 內的 Chocolatey 步驟4")


官方文件顯示有這幾項

ChocolateyInstall

ChocolateyToolsLocation

ChocolateyLastPathUpdate

PATH (will need updated to remove)



不過我這邊移除的時候沒看到 ChocolateyLastPathUpdate 就是了






