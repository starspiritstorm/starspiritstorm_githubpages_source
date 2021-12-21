---

title: "Uninstall Chocolatey in Windows"
author: starspiritstorm
type: posts
date: 2021-12-21T16:20:52+08:00
draft: false
categories: ["chocolatey"]
tags: ["windows", "chocolatey"]
keywords: ["install chocolatey in windows"]

---


Refer to [Chocolatey Official website How to uninstall](https://docs.chocolatey.org/en-us/choco/uninstallation)


<!--more-->


## Uninstall Chocolatey

### Uninstall step 1. Delete Chocolatey Folder


Just directly delete the Chocolatey folder, ususally it will be at C:\ProgramData\chocolatey

if you are not sure where it is, you can use following command.

	$env:ChocolateyInstall 


![fig1](/figures_for_uninstall_chocolately/fig1.check_chocolately_location.png "Fig1. Check Chocolatey location")

![fig2](/figures_for_uninstall_chocolately/fig2.delete_chocolately_folder.png "Fig2. Delete Chocolatey Folder")


### Uninstall step 2. Remove chocolately from environment


After deleted the Chocolatey folder, you need to remove chocolately from environment.


![fig3](/figures_for_uninstall_chocolately/fig3.remove_chocolately_from_environment1.png "Fig3. Remove Chocolatey from Environment step 1")

![fig4](/figures_for_uninstall_chocolately/fig4.remove_chocolately_from_environment2.png "Fig4. Remove Chocolatey from Environment step 2")

![fig5](/figures_for_uninstall_chocolately/fig5.remove_chocolately_from_environment3.png "Fig5. Remove Chocolatey from Environment step 3")

![fig6](/figures_for_uninstall_chocolately/fig6.remove_chocolately_from_environment4.png "Fig6. Remove Chocolatey from Environment step 4")


The Official docs indicate there are four following subject need to be removed.


ChocolateyInstall

ChocolateyToolsLocation

ChocolateyLastPathUpdate

PATH (will need updated to remove)


But when I doing the remove, I didn't see the ChocolateyLastPathUpdate.






