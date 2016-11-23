---
layout: post
title: All About Sublime Text 3
subtitle: My Own Guide to Setup ST3
tags: [environment, know-how]
bigimg: /img/path.jpg
---


### 1. Package Control
[Package Control](https://packagecontrol.io/) is the prerequisite for other installations. Use ctrl+shift+p(Ubuntu)/command+shift+p(MacOS) to access from [Sublime Text 3](http://www.sublimetext.com/3)

### 2. Functional Packages

#### General IDE Intelligence
* [SublimeLinter](https://packagecontrol.io/packages/SublimeLinter) - For [code linting](http://stackoverflow.com/questions/8503559/what-is-linting)
* [SublimeCodeIntel](https://packagecontrol.io/packages/SublimeCodeIntel) - For code auto-complete
* [BracketHighlighter](https://packagecontrol.io/packages/BracketHighlighter) - For bracket highlight

#### Markdown Preview
* [Markdown Preview](https://packagecontrol.io/packages/Markdown%20Preview)

#### Latex Preview
* [LaTeXTools](https://packagecontrol.io/packages/LaTeXTools)
* [LaTeXing](https://packagecontrol.io/packages/LaTeXing)

### 3. Theme 
* [Soda](https://packagecontrol.io/packages/Theme%20-%20Soda)
* [Seti_UX](https://packagecontrol.io/packages/Seti_UX)

### 4. Color-Scheme
* [Tomorrow](https://packagecontrol.io/packages/Tomorrow%20Color%20Schemes)
* [Seti_UI](https://packagecontrol.io/packages/Seti_UI)

### 5. Code Linting 

#### For Python:

1. Install [Sublime​Linter-pylint](https://packagecontrol.io/packages/SublimeLinter-pylint) for Python, which provides an interface to [pylint](https://www.pylint.org/). 
2. Install [pylint](https://www.pylint.org/):

```shell
sudo apt-get install pylint
```

3. Check pylint version, the apt-get method installs pylint for Python 2 by default:

```shell
less $(which pylint)
```

4. Install pylint specifically for Python 3:

```shell
sudo pip3 install pylint
```

5. Create a pylint configuration

```shell
pylint --generate-rcfile > ~/.pylintrc
```

6. To avoid pylint to throw some warnings, edit .pylintrc such as:

```shell
[MESSAGES CONTROL]
disable = F0401, W0141, R0914
#W0141:Used builtin function
#F0401:Unable to import
#R0914:Too many local variables
```

All pylint messages are listed [here](http://pylint-messages.wikidot.com/)

#### For Cpp:
1. Install [SublimeLinter-cpplint](https://github.com/SublimeLinter/SublimeLinter-cpplint) for C++, which provides an interface to [cpplint](https://pypi.python.org/pypi/cpplint)

2. Install [cpplint]:

```shell
sudo pip install cpplint
```

3. Check cpplint version

```shell
less $(which cpplint)
```

### 6. Configuration

#### User Settings
In Preferences -> Setting-User, update the following syntax:

```shell
{
	"bold_folder_labels": true,
	"caret_style": "phase",
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
	"theme": "Seti.sublime-theme"
}
```
Color scheme can be directly selected in Preferences -> Color Scheme


#### Key Bindings
In Preference -> Key Bindings-User, update the following:

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