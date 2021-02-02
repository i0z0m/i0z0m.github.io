===========================
make GitHub Pages by Sphinx
===========================

   :Date: 2021/02/02 19:20

usage
=======

.. code:: sh

   $ pip install --user sphinx ghp-import

.. code:: sh

   $ vim .zshenv
   export PATH=$PATH:~/.local/bin

.. code:: sh

  $ sphinx-quickstart
  $ make html

change theme
============

.. code:: sh

  $ pip install --user sphinx_rtd_theme

edit source/conf.py

.. code:: sh

  html_theme = 'sphinx_rtd_theme'
  favicon = 'favicon.ico'
  pygments = 'monokai'

hosting to GitHub Pages
=======================

.. code:: sh

  $ git init
  $ git add *
  $ git commit -m "first commit"
  $ git remote add origin git@github.com:i0z0m/i0z0m.github.io.git
  $ ghp-import -m "publish" docs/build/html
  $ ghp-import -p docs/build/html
