---
layout: post
title: Fonts in Ubuntu&MacOS
subtitle: My Own Guide to customize fonts
tags: [environment, know-how]
bigimg: /img/path.jpg
---


### Install fonts in Ubuntu
Choosing a nice font family usually helps coding a great deal. To install fonts fast and easy in Ubuntu, following the simple steps:

```shell
sudo mkdir ~/.fonts
sudo cp *.ttf ~/.fonts
sudo cp *.otf ~/.fonts
fc-cache -f -v
```

fc-cache is the command to manually rebuild the font cache, you can use the fonts anywhere immediately.

### Install fonts in MacOS

Installing fonts in MacOS is simpler, just copy the unzipped font files to ~/Library/fonts. In "Finder", click "Go" while pressing the "alt/option" key (⌥), the "Library" folder will appear.

### Font recommendation
* [Latin Modern Mono](http://www.fontsquirrel.com/fonts/Latin-Modern-Mono)
* [Ubuntu Mono](http://font.ubuntu.com/)