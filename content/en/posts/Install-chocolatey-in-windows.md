---

title: "Install Chocolatey in Windows"
author: starspiritstorm
type: posts
date: 2021-12-21T14:53:55+08:00
draft: false
categories: ["chocolatey"]
tags: ["windows", "chocolatey"]
keywords: ["install chocolatey in windows"]

---


For the purpose of installing hugo, we need to install package manager - Chocolatey for Windows.

This article will introduce how to install Chocolatey in Windows environment.


<!--more-->


## Install Chocolatey


First, go to the official web site of Chocolatey

[Chocolatey official site](https://chocolatey.org/)

[Chocolatey official site of install instruction](https://chocolatey.org/install)


![fig1](/images/install_chocolately/fig1.Instruction_of_install_chocolately.png "Fig1. Official web site of Chocolatey install instruction")


### Install step 1. Using System Administrator to open Windows Power Shell 

The command needs Windows Power Shell to execute, remember using System Administrator to open Windows Power Shell


![fig2](/images/install_chocolately/fig2.start_power_shell_with_admin.png "Fig2. Right click icon and select "Run as administrator" Windows Power Shell")


### Install step 2. Execute the command from Chocolatey install instruction

According to Chocolatey install instruction, if you have some authority control issue, you better go to the MSDN to check which authority level you required.

To check the setting of your Power Shell, you can using the following command.

	Get-ExecutionPolicy -list


![fig3](/images/install_chocolately/fig3.check_ExecutionPolicy.png "Fig3. Check your ExecutionPolicy")


Then you can run the "run following command"

![fig4](/images/install_chocolately/fig4.run_following_command.png)

	Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))


Note that command has written the "Bypass", so your Set-ExecutionPolicy will be set to Bypass, -Scope Process means only applied for Process

Using "AllSigned" will get multiple queries, it depends on your needs to decide using Bypass or AllSigned, I'm just lazy person so I just using Bypass.


![fig5](/images/install_chocolately/fig5.Bypass.png "Fig5. Bypass")

![fig6](/images/install_chocolately/fig6.AllSigned.png "Fig6. AllSigned")


After finished, you will see Chocolatey (choco.exe) is now ready.

![fig7](/images/install_chocolately/fig7.Chocolately_ready.png)



