# Week 2 Lab Report: Remote Access
## *How to log onto course-specific account on* `ieng6`
---

[Back To Home](https://ryan-truong.github.io/cse15l-lab-reports/)

---


**Step 1: Installing VS Code**

* First visit [Visual Studio Code](https://code.visualstudio.com/) and follow the instructions to download the program
* After it is open, a successful install and opening of the program should like look the following screenshot

![Image](vscode_example.png)

**Step 2: Remotely Connecting**
* First look up your course specific account for CSE15L [here](https://sdacs.ucsd.edu/~icc/index.php)
* Then under `Addional Accounts` you will see buttons that represent course specific accounts

    * The button labeled `cs15lwi22xxx` where `xxx` is replaced by your specific characters represents your account for this class and will be used to log in
    
        *Note: wi22 is subject to change for the quarter you take it in*
    
    * **IMPORTANT**: To successfully follow the next steps, you must change your password by clicking `change your password` and following the steps

* Now in VSCode, click on `Terminal` found at the top of the window, and then `New Terminal`
* Type into the Terminal that opens: `ssh cs15lwi22xxx@ieng6.ucsd.edu` where `cs15lwi22xxx` is your course specific account
* It will then ask you `Are you sure you want to continue connecting` type `Yes` and enter your `Password`
* If done sucessfully your terminal will look like the following screenshot

![Image](ssh_example.png)


**Step 3 Trying Some Commands**

**Step 4: Moving Files with** `scp`

**Step 5: Setting an SSH key**

**Step 6: Optimize Remote Running**
