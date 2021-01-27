---
title: "Pyenv - Installing Python on MacOS"
date: 2021-01-27T22:09:18+01:00
tags: [python, sysadmin, snippets]
---

After fiddeling with different and various installments and versions of Python on MacOS, I stumbled over the article [The right and wrong way to set Python 3 as default on a Mac](https://opensource.com/article/19/5/python-3-default-mac) - which works like a charm!

In short:
- brew install pyenv
- pyenv install 3.9.1 - the Python version you want to install
- pyenv global 3.9.1 - set it as preferred version
- make pyenv known to your login shell ($HOME/.zshrc)
    if command -v pyenv 1>/dev/null 2>&1; then
 	 eval "$(pyenv init -)"
    fi
- use python and pip
