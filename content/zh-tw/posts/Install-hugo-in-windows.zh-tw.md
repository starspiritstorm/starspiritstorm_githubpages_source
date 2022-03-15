---

title: "在 Windows 環境下安裝 Hugo"
author: starspiritstorm
type: posts
date: 2022-03-15T08:00:00+08:00
draft: false
categories: ["hugo"]
tags: ["windows", "hugo"]
keywords: ["install hugo in windows"]

---


安裝 Hugo。


<!--more-->


## 安裝 Hugo


裝完chocolatey後，開啟 powershell，然後輸入

	choco install hugo -confirm

![fig1](/images/install_hugo/fig1.Install_hugo.png "Fig1. 安裝 hugo")


安裝好 hugo 後，安裝 hugo extended，輸入 

	choco install hugo-extended -confirm


![fig2](/images/install_hugo/fig2.Install_hugo_extended.png "Fig2. 安裝 hugo extended")


結束。


## 檢查 Hugo 版本及反安裝 Hugo


兩個簡單指令。

檢查版本

	hugo version


反安裝Hugo

	choco uninstall hugo


![fig3](/images/install_hugo/fig3.Check_hugo_version&Uninstall_hugo.png "Fig3. 檢查hugo版本與反安裝hugo")





