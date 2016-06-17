---
title: 'Fedora, rxvt, powerline, tmux and vim'
date: 2016-06-17 23:00:28
tags:
- fedora
- rxvt
- powerline
- tmux
- vim
- linux
- colors
- dotfiles
- fonts
---
Here's how I set up rxvt on Fedora 23 with powerline in tmux and vim.

## Packages

``` bash
$ sudo dnf -y install vim tmux rxvt-unicode-256color-ml xclip
```

I needed to use the ml variant of rxvt in order to get the arrows in the powerline font working, and I needed the 256color variant to get the colors right.
xclip is needed for the clipboard integration in tmux, i.e. being able to do `ctrl-t ]` to paste from the clipboard and placing the selected text in the clipboard.

## Dotfiles

I keep my dotfiles in git. I've made a script to symlink .vimrc, .tmux.conf, etc. into place.
If you already have your own dotfiles that you're happy with you can skip this step.

``` bash
$ cd ~
$ git clone https://github.com/kabo/dotfiles.git
$ cd dotfiles
$ git submodule update --init --recursive
$ ./makesymlinks.sh
```

After this vim is going to complain about ycm not being correctly installed. [Instructions on that](https://github.com/Valloric/YouCompleteMe#fedora-linux-x64).
Here's what I do.

``` bash
$ sudo dnf -y groupinstall "Development Tools"
$ sudo dnf -y install clang cmake golang
$ curl --silent --location https://rpm.nodesource.com/setup_6.x | sudo bash -
$ sudo dnf -y install nodejs
$ cd ~/dotfiles/vim/bundle/YouCompleteMe
$ ./install.py --clang-completer --tern-completer --gocode-completer
```

## Font

Now let's get a patched font. I like DejaVu, there's more [here](https://github.com/powerline/fonts).

``` bash
$ cd ~/Downloads
$ wget 'https://github.com/powerline/fonts/raw/master/DejaVuSansMono/DejaVu%20Sans%20Mono%20for%20Powerline.ttf'
$ sudo cp DejaVu\ Sans\ Mono\ for\ Powerline.ttf /usr/share/fonts/dejavu/DejaVuSansMonoForPowerline.ttf
```

## Powerline

I like using [tmux-powerline](https://github.com/erikw/tmux-powerline), it doesn't feel bloated. It's included as a submodule in my dotfiles. Let's generate a new theme.

``` bash
$ cd ~/dotfiles/tmux-powerline/themes
$ cp default.sh mytheme.sh
$ vim mytheme.sh # I like to disable all segments except tmux_session_info, hostname, pwd and load
$ cd ..
$ ./generate-rc.sh
$ cd ~
$ mv .tmux-powerline{.default,}
$ vim .tmux-powerline # change the theme to mytheme
```

## tmux

In order to autostart into tmux  when you start rxvt, add these two lines at the end of your `.bashrc`

``` bash
[[ $- != *i* ]] && return
[[ -z "$TMUX" ]] && exec tmux
```

OK, time to start rxvt
``` bash
$ urxvt256c-ml
$ xrdb -load .Xresources
$ tmux source-file .tmux.conf
```

## Keyboard shortcuts

I like being able to start the terminal with `ctrl-alt-T`. To do that, edit `~/.config/openbox/lxde-rc.xml` and add the following keybinding.

``` xml
<keybind key="C-A-T">
  <action name="Execute">
    <command>urxvt256c-ml</command>
  </action>
</keybind>
```

Now log out and log back in. Hopefully you'll be able to press `ctrl-alt-T` and get an rxvt that looks like this.

![](/images/urxvt_037.png)

And if you type `vim` you should get this.

![](/images/urxvt_038.png)
