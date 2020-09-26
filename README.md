EHRbase Documentation
========

How to view it?
---------------

Visite http://docs.ehrbase.org/en/latest/ to view the latest version of the documentation rendered as html

How to build it?
---------------

When you want to build the documentation locally, we recommend to use the latest version of Sphinx. Download from http://www.sphinx-doc.org/en/master/

Installation should be straight forward.

After successful installation, add also the HTML Domain by executing: `pip install sphinxcontrib-httpdomain`

> If you get the error `ModuleNotFoundError: No module named 'sphinx_rtd_theme'` then install the missing theme separately:
> 
> `pip install sphinx_rtd_theme`

To build the project (for example as html pages), type:

`make html`


Linux installation and build
----------------------------

Prerequisite: sphnix needs python 3.6+, if you have python 2 installed, it won't work.

Installation:

$ apt install python3-sphinx
$ make html

If the command above returned an error, try to install a newer version of sphinx, the installed version is 1.6.7.

$ pip install "Sphinx=2.4.3"
$ python -m pip list | grep Sphinx
$ make html

If the command above returned a module not found error for pip install.

$ pip install sphinx_rtd_theme
$ python -m pip list | grep sphinx
$ make html

The command above should build the documentation in the build/ folder.

Note: everytime `make html` is executed, before execure `rm -R build` to force updating all the files.


License
-------
EHRbase Documentation uses the Apache 2 license.
