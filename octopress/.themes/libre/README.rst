#################################
K-Theme-Libre
#################################

This theme is further customized for `K`_.

.. _`K`: https://github.com/KellyChan/k

*******************
How to use
*******************

::

    # download platform
    $ git clone https://github.com/KellyChan/k.git

    # download theme
    $ cd k/octopress/.theme
    $ ls
    $ git clone https://github.com/KellyChan/k-theme-libre.git

    # install theme
    $ cd k/octopress
    $ rake install['libre']
    $ rake generate
    $ jekyll serve


******************
How to update
******************

::

    $ cp -R octopress/sass octopress/.theme/libre/sass
    $ cp -R octopress/source octopress/.theme/libre/source
