---
title:  "Installing Extraterm on Ubuntu 19.10 (Updated)"
date: 2020-04-04 08:00
---
** This is an updated version for Ubuntu 20.04 and extraterm 0.59.2 (current Date is 07.06.2021)

I recently purchased a desktop PC (mostly for gaming and audio editing), after several days of doing that I installed an Ubuntu dist and I am currently in the process of making it a dev machine as well. In this post I am going to talk about an alternative to default Ubuntu Terminal - Extraterm.

I am missing the iTerm + zsh + oh-my-zsh combination from my Mac machine and I found out that Extraterm is a cool alternative to the setup I have on the other operating system.

In order to install it you need to follow those several steps

- Head to the [Extraterm official website](https://extraterm.org/){:target="blank"}
- Go to the [Installation Section](https://extraterm.org/guide.html#installation){:target="blank"}
- Download the zip file for Ubuntu on the github releases page. In my case I had to download the "extraterm-0.59.2-linux-x64.zip"

Once I did that I decide to place the extracted files in ~/Applications folder. At that point in order to test whether the Extraterm is working you can

- open a terminal
- then navigate to the folder and run the application

```
cd ~/Applications/extraterm-0.59.2-linux-x64 && ./extraterm
```

![](/assets/images/extraterm.png)

You should be seeing the Extraterm running, the problem with this is that you need to go to the default Terminal and run it from there which is not the perfect user experience. I want to change two things, #1 the obvious "not going through the terminal" and #2 the use of the Ubuntu keyboard settings (Ctr + Alt + T) to run the Extraterm.

To solve #1 you will need to copy the .desktop file to /usr/share/applications for any user to be able to use it (you will need administrator permissions) or to ~/.local/share/applications if you just want it to be available for one user. In my case I only have one user so let's go for a local installation

```
cp ~/Applications/extraterm-0.59.2-linux-x64/extraterm.desktop ~/.local/share/applications/.
```

Open it with a text editor and change the Exec directive to

```
Exec=bash -c "~/Applications/extraterm-0.59.2-linux-x64/extraterm"
```

Save it and now you can try to search for Extraterm in your Quickfind. Now in order to solve #2 we need to edit some system cofigurations:

```
gsettings list-recursively | grep terminal
```

This one will list all of the terminal related configuration options and we need to change the default application for the desktop terminal

```
gsettings set org.gnome.desktop.default-applications.terminal exec 'bash -c "~/Applications/extraterm-0.59.2-linux-x64/extraterm"'
```

Voalá!!! Everything should be setup and Ctr + Alt + T should open the Extraterm program. Head to the [official user guide](https://extraterm.org/guide.html){:target="blank"} in order to see what else Extraterm is capable of.
