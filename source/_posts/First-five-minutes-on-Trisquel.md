---
title: First five minutes on Trisquel
tags:
  - linux
  - trisquel
  - setup
  - vim
  - rxvt
  - tmux
  - powerline
  - lxde
date: 2016-06-22 22:03:17
---

I install [Trisquel](https://trisquel.info/) Mini as it feels a lot snappier than regular Trisquel, and I'm really happy with LXDE.
``` bash
$ sudo aptitude update && sudo aptitude -y full-upgrade
$ sudo aptitude install -y xfce4-volumed synapse htop tmux vim-gtk icecat abrowser build-essential xclip keepassx shutter clang cmake golang icedove enigmail rxvt-unicode-256color openjdk-7-jdk curl git x11-xserver-utils python-dev xscreensaver xscreensaver-gl-extra xscreensaver-data-extra python-pip trisquel-codecs libssl-dev xfce4-clipman
$ sudo pip install awscli markupsafe ansible
$ curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
$ sudo aptitude install -y nodejs
$ sudo npm install hexo -g # so I can continue with this blog :)
```

Then I fix up [my dotfiles](https://github.com/kabo/dotfiles).
``` bash
$ cd dotfiles
$ ./makesymlinks.sh
$ ./installfont.sh
$ ./generatetmuxpowerline.sh
$ cd vim/bundle/YouCompleteMe
$ ./install.py --clang-completer --tern-completer --gocode-completer
$ urxvt
```

In this new terminal I type
``` bash
$ xrdb -load .Xresources
```

Then I exit, and open urxvt again, this time with a new look (hopefully). Now I add the following to the end of my `.bashrc` and restart urxvt once again.
``` bash
[[ $- != *i* ]] && return
[[ -z "$TMUX" ]] && exec tmux
```

After reading this blog post on [how and why to log your entire bash history](https://spin.atomicobject.com/2016/05/28/log-bash-history/), I also add this to my `.bashrc`.
``` bash
export PROMPT_COMMAND='if [ "$(id -u)" -ne 0 ]; then echo "$(date "+%Y-%m-%d.%H:%M:%S") $(pwd) $(history 1)" >> ~/.logs/bash-history-$(date "+%Y-%m-%d").log; fi'
```

Next I ensure that these lines are in `~/.config/openbox/trisquel-mini-rc.xml`. This allows me to open a new terminal with `ctrl-alt-T`, lock the screen with `ctrl-alt-L` and tile the current window with `super-alt-left/right/up/down`.
``` xml
<keybind key="C-A-T">
  <action name="Execute">
    <command>urxvt</command>
  </action>
</keybind>
<keybind key="C-A-L">
  <action name="Execute">
    <command>lxlock</command>
  </action>
</keybind>
<keybind key="W-A-Left">
  <action name="UnmaximizeFull"/>
  <action name="MoveResizeTo">
    <width>50%</width>
  </action>
  <action name="MaximizeVert"/>
  <action name="MoveResizeTo">
    <x>0</x>
    <y>0</y>
  </action>
</keybind>
<keybind key="W-A-Right">
  <action name="UnmaximizeFull"/>
  <action name="MoveResizeTo">
    <width>50%</width>
  </action>
  <action name="MaximizeVert"/>
  <action name="MoveResizeTo">
    <x>-0</x>
    <y>0</y>
  </action>
</keybind>
<keybind key="W-A-Up">
  <action name="UnmaximizeFull"/>
  <action name="MoveResizeTo">
    <height>50%</height>
  </action>
  <action name="MaximizeHorz"/>
  <action name="MoveResizeTo">
    <x>0</x>
    <y>0</y>
  </action>
</keybind>
<keybind key="W-A-Down">
  <action name="UnmaximizeFull"/>
  <action name="MoveResizeTo">
    <height>50%</height>
  </action>
  <action name="MaximizeHorz"/>
  <action name="MoveResizeTo">
    <x>0</x>
    <y>-0</y>
  </action>
</keybind>
```

I then create my custom keyboard layout which you can read about in my previous blog post. But in Trisquel, instead of adding the line to my `.bashrc`, I create a file with the command in it and then autostart it by creating the file `~/.config/autostart/keyboard.desktop`:
``` ini
[Desktop Entry]
Exec=/home/kabo/keyboardfix.sh
Type=Application
Name=KeyboardFix
```

Then I log out and log back in to have everything take effect.

Now I've got the things I need to start working.
