---
title: "Making Poetry 1.1.12 work in Python 3.10"
date: 2021-12-28T19:31:47+01:00
draft: true
---

To make Poetry 1.1.12 work with Python 3.10 on macOS Big Sur, I needed to install the following dependencies:

`pip install cleo tomlkit poetry.core requests cachecontrol cachy html5lib pkginfo virtualenv lockfile`

Thanks to [@hoishing](https://github.com/python-poetry/poetry/issues/553#issuecomment-994303497) for the tip.