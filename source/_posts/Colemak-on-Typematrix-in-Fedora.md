---
title: Custom Colemak on Typematrix in Fedora
tags:
  - fedora
  - colemak
  - typematrix
  - keyboard-layout
  - qwerty
  - dvorak
  - linux
  - xkb
date: 2016-06-18 22:32:29
---

I use a [Typematrix keyboard](http://typematrix.com/2030/features.php) with a black universal 101 skin (pictured below).

![](/images/2030-skin-025-b-universal101-860x360.png)

I use the [colemak layout](https://colemak.com/) so the characters are laid out like this, but with a few tweaks.

![](/images/2030-skin-050-c-colemak-860x360.png)

The reason I wasn't satisfied with Swedish QWERTY was that the keyboard layout was horrendous to type code with. Most programming languages are made by Americans, and it really shows when it comes to what characters they use. Chars like `[` require one keypress on American QWERTY, and two keys on Swedish QWERTY. `{` requires three keys to be pressed.

So I wanted to get away from the Swedish layout. I could've just switched to an American layout, but if I'm going to do a change I may as well look into further optimization. I had tried dvorak previously, but that was to big of a change. Then I found colemak, which keeps a lot of shortcuts in the same place. It was much easier to learn.

The Typematrix has a nifty feature where it let's you emulate different layouts in the keyboard. The computer thinks there's a US QWERTY keyboard attached, but the Typematrix converts my keystrokes from colemak to qwerty. This means that the keyboard on the laptop, which is QWERTY still works as expected.

So I used my Typematrix keyboard and the US colemak layout, and coding went very well. But there's one thing missing; I can't type Å, Ä or Ö.

So here's how I tweak the US QWERTY layout to include Å, Ä and Ö in Fedora 23.

``` bash
$ cd /usr/share/X11/xkb/symbols
$ sudo cp us us_backup
$ sudo vim us
```

Here I add the following after `xkb_symbols "euro"` section.

```
partial alphanumeric_keys
xkb_symbols "kabo" {

    name[Group1]= "English (US, kabo)";
    include "us(basic)"
    include "eurosign(5)"

    key <AD09> { [	   o,          O,    odiaeresis,       Odiaeresis ] };
    key <AC10> { [ semicolon,	   colon,         aring,            Aring ] };
    key <AC11> { [ apostrophe,	quotedbl,    adiaeresis,       Adiaeresis ] };

    include "level3(ralt_switch)"
};
```

Then I run this command.

``` bash
$ setxkbmap -layout "us,se" -variant "kabo," -option "terminate:ctrl_alt_bksp,grp:shifts_toggle"
```

Now I can type with my Typematrix in my special US mode (with Å mapped to `altgr-;`, Ä mapped to `altgr-'`, and Ö mapped to `altgr-o`), and the Swedish QWERTY on my laptop works reasonably well. If I want to I can switch over to the Swedish QWERTY simply by pressing both shift buttons. Awesome! :)

To have this setup permanently I just add that command to my `.bashrc`.
