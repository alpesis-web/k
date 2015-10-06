#######################
Theming
#######################

- If change a new theme, copy the theme-files to .theme/theme_name, then the files will be replaced in the main sources.
- If create a new theme, modify the sass/sources files in the main sources, and then copy them in a new repo.


*******************
Creating a theme
*******************

::

    $ cd octopress/.theme
    $ cp -R classic theme_name
    $ rake install['theme_name']
    $ rake generate

*********************
Source
*********************

*********************
Sass
*********************

file skel

::

    ./screen.scss
        |---- custom/
        |---- _base.scss
        |        |---- base/
        |                |---- _utilities.scss            # mixins
        |                |---- _solarized.scss            # color variables
        |                |---- _theme.scss
        |                |---- _typography.scss           # fonts
        |                |---- _layout.scss
        |
        |---- _partials.scss
        |        |---- partials/
        |---- plugins/
  
