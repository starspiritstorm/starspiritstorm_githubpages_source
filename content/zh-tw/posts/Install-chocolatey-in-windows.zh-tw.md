---

title: "在 Windows 環境下安裝 Chocolatey"
author: starspiritstorm
type: posts
date: 2021-12-21T14:53:55+08:00
draft: false
categories: ["chocolatey"]
tags: ["windows", "chocolatey"]
keywords: ["install chocolatey in windows"]

---


為了安裝 Hugo，需要先安裝 Windows 用的套件管理軟體 Chocolatey

本篇文章會介紹如何在 Windows 環境下安裝 Chocolatey

<!--more-->


## 安裝 Chocolatey


首先，要安裝 Chocolatey 就是要去他的官方網站

[chocolatey 官方網站](https://chocolatey.org/)

[chocolatey 安裝頁面](https://chocolatey.org/install)


![fig1](/figures_for_install_chocolately/fig1.Instruction_of_install_chocolately.png "Fig1. Chocolatey 的官方網頁安裝步驟")


### 安裝步驟 1. 用"管理者權限"開啟 Windows Power Shell 

安裝頁面的指令需要用 Windows Power Shell  執行，Windows Power Shell 記得要用"管理者權限"開啟。


![fig2](/figures_for_install_chocolately/fig2.start_power_shell_with_admin.png "Fig2. 右鍵點選 Windows Power Shell 並用管理者權限開啟 ")


### 安裝步驟 2. 執行 Chocolatey 官網提供的指令


根據 Chocolatey 的安裝指示，在安裝 Chocolatey 之前，最好先去 MSDN 確定一下你要的權限 level

檢查目前 Power Shell 的設定可以用這行指令

	Get-ExecutionPolicy -list


![fig3](/figures_for_install_chocolately/fig3.check_ExecutionPolicy.png "Fig3. 檢查權限")


接下來就跑 "run following command" 的指令

![fig4](/figures_for_install_chocolately/fig4.run_following_command.png)

	Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))


注意它裡面有寫 Bypass，所以你的 Set-ExecutionPolicy 會被設定成 Bypass，-Scope Process 這段是表示設定只套用於 Process

用 AllSigned 會有一堆詢問，看每個人的習慣自行決定要用 Bypass 還是 AllSigned，我比較懶所以就直接用 Bypass 了


![fig5](/figures_for_install_chocolately/fig5.Bypass.png "Fig5. Bypass")

![fig6](/figures_for_install_chocolately/fig6.AllSigned.png "Fig6. AllSigned")


最後裝完以後就會看到 Chocolatey (choco.exe) is now ready

![fig7](/figures_for_install_chocolately/fig7.Chocolately_ready.png)



