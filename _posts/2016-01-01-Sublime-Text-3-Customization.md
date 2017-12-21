---
layout: post
title: Sublime Text 3 Customization
subtitle: A Personal Guide 
tags: [Editor]
bigimg: /img/path.jpg
---


### 1. Install Package Control
[Package Control](https://packagecontrol.io/) is the prerequisite to install all packages. After installing Package Control, use ctrl+shift+p(Ubuntu)/command+shift+p(MacOS) to gain access.

### 2. Install Packages

#### General IDE Intelligence
* [SublimeCodeIntel](https://packagecontrol.io/packages/SublimeCodeIntel) - For code auto-complete
* [BracketHighlighter](https://packagecontrol.io/packages/BracketHighlighter) - For bracket highlight
* [SideBarEnhancements](https://packagecontrol.io/packages/SideBarEnhancements) - For sidebar enhancement
* [SublimeLinter](https://packagecontrol.io/packages/SublimeLinter) - For [code linting](http://stackoverflow.com/questions/8503559/what-is-linting)

#### Language Intelligence
* [Python3](https://packagecontrol.io/packages/Python%203)
* [Markdown Preview](https://packagecontrol.io/packages/Markdown%20Preview)
* [LaTeXTools](https://packagecontrol.io/packages/LaTeXTools)
<!--* [LaTeXing](https://packagecontrol.io/packages/LaTeXing)(less popular)-->
* [Emmet](https://packagecontrol.io/packages/Emmet)

### 3. Intall Theme&Color-Scheme 
* [Seti_UI](https://packagecontrol.io/packages/Seti_UI)
* [Seti_UX](https://packagecontrol.io/packages/Seti_UX)
<!--* [Theme - Soda](https://packagecontrol.io/packages/Theme%20-%20Soda)-->
<!--* [Material Theme](https://packagecontrol.io/packages/Material%20Theme)-->
<!--* [Tomorrow Color Schemes](https://packagecontrol.io/packages/Tomorrow%20Color%20Schemes)-->
### 4. Setup Code Linting

#### For Python:

Install [Sublime​Linter-pylint](https://packagecontrol.io/packages/SublimeLinter-pylint) for Python, which provides an interface to [pylint](https://www.pylint.org/). 

Install [pylint](https://www.pylint.org/)

```shell
sudo apt-get install pylint
```
It installs pylint for Python 2 by default. For Python 3:

```shell
sudo pip3 install pylint
```

To check pylint version,

```shell
less $(which pylint)
```

To create a pylint configuration

```shell
pylint --generate-rcfile > ~/.pylintrc
```

To avoid pylint to throw some warnings, edit .pylintrc such as:

```shell
[MESSAGES CONTROL]
disable = F0401, W0141, R0914
#W0141:Used builtin function
#F0401:Unable to import
#R0914:Too many local variables
```

All pylint messages are listed [here](http://pylint-messages.wikidot.com/)

#### For C++:
Install [SublimeLinter-cpplint](https://github.com/SublimeLinter/SublimeLinter-cpplint) for C++, which provides an interface to [cpplint](https://pypi.python.org/pypi/cpplint)

Install [cpplint](https://pypi.python.org/pypi/cpplint):

```shell
sudo pip install cpplint
```

To check cpplint version

```shell
less $(which cpplint)
```

### 5. Configuration

#### User Settings
In Preferences -> Setting, update the following syntax:

```shell
{
	"bold_folder_labels": true,
	"caret_style": "phase",
	"color_scheme": "Packages/Python 3/Gloom.tmTheme",
	"fade_fold_buttons": false,
	"font_face": "Latin Modern Mono",
	"font_size": 18.0,
	"highlight_line": true,
	"ignored_packages":
	[
		"Vintage"
	],
	"line_padding_bottom": 1,
	"line_padding_top": 1,
	"theme": "Seti_orig.sublime-theme"
}
```
Color scheme can be directly selected in Preferences -> Color Scheme
Theme can be directly selected in Preferences -> Theme

#### Key Bindings

In Preferences -> Key Bindings, update the following:

##### For Ubuntu

```shell
[
	{"keys": ["ctrl+alt+c"], "command": "exec", "args": {"kill": true}},
	{"keys": ["ctrl+alt+r"], "command": "reindent" , "args": { "single_line": false }},
	{"keys": ["ctrl+alt+m"], "command": "markdown_preview", "args": {"target": "browser", "parser":"markdown"}},
]
```

##### For MacOS

```shell
[
	{"keys": ["ctrl+command+c"], "command": "exec", "args": {"kill": true}},
	{"keys": ["ctrl+command+r"], "command": "reindent" , "args": { "single_line": false }},
	{"keys": ["ctrl+command+m"], "command": "markdown_preview", "args": {"target": "browser", "parser":"markdown"}},
]
```