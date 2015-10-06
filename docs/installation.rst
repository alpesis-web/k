######################
Installation
######################


*****************
Prequisitions
*****************

Installing the dependencies:

- ubuntu 14.04
- node.js
- rvm
- ruby
- gem
- bundler
- rake

node.js

::

    $ sudo apt-get install nodejs

rvm and ruby

::

    $ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
    $ \curl -sSL https://get.rvm.io | bash -s stable 
    $ rvm install 2.1.1

Octopress

::

    $ git clone git://github.com/imathis/octopress.git octopress
    $ cd octopress
    $ gem install bundler
    $ bundle install
    $ rake install


***************
Run
***************

::

    $ rake generate
    $ jekyll serve


site: http://0.0.0.0:4000
