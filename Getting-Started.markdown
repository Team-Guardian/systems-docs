Team Guardian makes heavy use of Linux and Python, both of which are very widely used in the software industry and are beneficial to learn. Git is used to deal with code that is worked on by multiple people and to provide a history of the code in the event that and earlier version is needed. Becoming familiar with these tools will allow you to make meaningful contributions to the vision system software.

1. [Linux command line tutorial](http://linuxcommand.org/lc3_learning_the_shell.php)
 * [Alternative tutorial](https://learnpythonthehardway.org/book/appendixa.html)
2. [Linux command line practice](https://www.shortcutfoo.com/app/dojos/command-line)
3. [Python tutorial and practice](https://www.codecademy.com/learn/python)
4. [Git interactive tutorial](https://try.github.io/)
5. [Git branching interactive tutorial](http://learngitbranching.js.org/)

# Setting Up a Development Environment

If you don't use Linux already on your computer, the easiest way to start using it without affecting your main operating system is to use a virtual machine. A VM allows you to run another OS in an application on top of your existing OS. If you already use Linux, you can choose to set up the codebase on your existing system natively or use a VM.

1. Download free virtualization software such as [VirtualBox](https://www.virtualbox.org/wiki/Downloads) (Windows, Mac OS, Linux) or [VMWare Workstation Player](https://my.vmware.com/web/vmware/free#desktop_end_user_computing/vmware_workstation_player/12_0) (Windows, Linux) and install it.
2. Download a Linux installation ISO. Recommended: [Linux Mint Cinnamon](https://www.linuxmint.com/edition.php?id=217). Alternative: [Ubuntu](https://www.ubuntu.com/download/desktop).
3. In your virtualization software, create a new virtual machine using the Linux ISO as the install media. A virtual disk size of 30 GB or more is sufficient. You can use the dynamic disk option so that only the required space is used on your hard drive instead of the full virtual disk size immediately.

# Getting the Codebase

1. On GitLab, navigate to your [SSH keys page](https://csil-git2.cs.surrey.sfu.ca/profile/keys).
2. If you don't already have an SSH key (try `cat ~/.ssh/id_rsa.pub` in a terminal to find out), use the instructions [here](https://csil-git2.cs.surrey.sfu.ca/help/ssh/ssh) to generate one.
3. In a terminal, type the following: `cat ~/.ssh/id_rsa.pub`
4. Copy the output to the clipboard.
5. On the SSH Keys page, click Add SSH Key. Paste the copied key into the Key box and give the key a title (maybe your computer name). Click "Add key". You can now work on the code without needing to enter your GitLab password.
6. In a terminal, create and navigate to the directory where you want the codebase to reside. For example: `mkdir ~/uav; cd ~/uav`
7. Type the following: `git clone ssh://git@csil-git2.cs.surrey.sfu.ca:24/Guardian/vision-system.git`
8. The codebase is cloned to "vision-system" in the current directory.
