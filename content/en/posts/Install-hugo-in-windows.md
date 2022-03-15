---

title: "Install Hugo in Windows"
author: starspiritstorm
type: posts
date: 2022-03-15T08:00:00+08:00
draft: false
categories: ["hugo"]
tags: ["windows", "hugo"]
keywords: ["install hugo in windows"]

---


Installing hugo.


<!--more-->


## Install Hugo


After finished installing chocolatey, just simply open up powershell, and type

	choco install hugo -confirm

![fig1](/images/install_hugo/fig1.Install_hugo.png "Fig1. Install hugo")


Then install hugo extended, type 

	choco install hugo-extended -confirm


![fig2](/images/install_hugo/fig2.Install_hugo_extended.png "Fig2. Install hugo extended")


And done.


## Check Hugo version and Uninstall Hugo


Two simple command.

Check version

	hugo version


Uninstall hugo

	choco uninstall hugo


![fig3](/images/install_hugo/fig3.Check_hugo_version&Uninstall_hugo.png "Fig3. Check_hugo_version&Uninstall_hugo")


