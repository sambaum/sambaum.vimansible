Vim editor custom configuration through Ansible
===============================================

This role aims to provide a custom configuration for the excellent
[Vim editor](http://www.vim.org) by using Ansible automation.

It's mainly based on a plugin manager for Vim and a set of plugins previously
selected by people very proficient with that editor. Also, the target users are
software developers and system administrators that know Vim and want to use it
in a more efficient way and save a valuable time, by letting Ansible take care
of the hard and repetitive job.

Features
--------

- Uses [Vundle plugin manager](https://github.com/VundleVim/Vundle.vim).
I think it can be changed to also work with other plugins managers, like
NeoBundle
- Assumes the operating system is Debian based and that APT packages are
available. With some changes other operating systems can be supported too

Usage
-----

From your Ansible playbook include this role (*vimansible*) and optionally set
the appropriate values for mainly these variables:

- `default_plugins`: a default list of Vim plugins that I consider useful
- `more_plugins`: another list to add more plugins to the previous default list
- `excluded_plugins`: another list to exclude plugins from the lists defined by
the `default_plugins` and `more_plugins` variables

The `default_plugins` and `more_plugins` list variables contain one or more
dictionary entries and both variables follow this pattern:

```YAML
    more_plugins:
      - name: name_of_plugin
        source: source_of_plugin
        comment: A brief informative note about the plugin
        required_debs:
          - a_package
          - b_package
        initializations:
          - set x=y
          - 'set x="y"'
        shell_commands:
          - dir: /some_dir_path/
          - cmd: ./some_script.sh
```

The `initializations` key contains a list of lines with valid syntax for the
Vim configuration files. The `shell_commands` key contains a list of possible
Bash commands needed by the plugin (see, for example, the case of the useful
[YouCompleteMe plugin](https://github.com/blueyed/YouCompleteMe))

The role file `defaults/main.yml` contains also more variables to adapt this
role to your needs, for example, to select the UI theme, background, user
configuration files and directory paths, names of OS packages, whether to
install OS packages or not, whether to install Vim plugins or not, whether to
apply needed shell commands related to some plugins or not, etc

Tip: installing operating system packages often requires administrative
privileges, but that could mean that the resulting Vim configuration directory
would end under the `root` user directory (e.g. `/root/.vim/`). To avoid this,
you could follow any of this options:

* Set the proper values for the variables `vim_home_dir_full_path`,
`vim_personal_config_file_full_path` and, optionally, `vim_home_dir_short_path`
* Run this role twice from a playbook:

  The first time with `sudo` for installing the needed packages with a command
  like this:

```Shell
   ansible-playbook -v --become --ask-become-pass --tags="packages" ~/my-vim-playbook/main.yml
```

   And a second time with a command like this:

```Shell
   ansible-playbook -v --skip-tags="packages" ~/my-vim-playbook/main.yml
```

* Set to `False` the value of the variable `install_packages_for_plugins` so
the declared operating system packages needed by some Vim plugins are not
installed by the Ansible task dealing with them in this role. You must find a
proper way to install those needed packages.

An important point is the visual appearance of Vim and this role considers that
through the `colorscheme` variable; you can set it to any value that is a valid
Vim color scheme. By default it's set to *bubblegum*, which, for me, looks fine
with a *light* background; however you can use any other you want. It's worth
mentioning some of the tested color schemes along with my subjective opinion:

* 256-grayvim: looks good without setting the Vim t_Co variable
* 256-grayvim: looks bad when setting the Vim t_Co variable to 256 and using a
light background
* 256-jungle: looks good with light background and without setting the Vim
t_Co variable
* 256-jungle: looks bad when setting the Vim t_Co variable to 256 and using a
light background
* mopkai: looks good with light background and with t_Co set to 256
* mopkai: looks good with dark background (except trailing whitespaces) and
with t_Co set to 256
* obsidian: looks good with light background and with t_Co set to 256
* gruvbox: looks good with dark background and t_Co set to 256
* hybrid: looks good with light background and t_Co set to 256
* badwolf: looks good with light background and t_Co set to 256
* Tomorrow: looks so-so with light background and t_Co set to 256
* Tomorrow-Night: looks so-so with light background and t_Co set to 256
* Tomorrow-Night-Bright: looks so-so with light background and t_Co set to 256
* Tomorrow-Night-Eighties: looks good with light background and t_Co set to 256
* Tomorrow-Night-Eighties: looks good with dark background (except trailing
whitespaces) and t_Co set to 256
* seoul256: looks so-so with light background and t_Co set to 256
* Chasing_Logic: looks good with light background (except highlighting of
matching html tags) and with t_Co set to 256
* radicalgoodspeed: looks so-so with light background and t_Co set to 256
* railscasts: looks so-so with light background and t_Co set to 256
* neverland: looks good with light background and t_Co set to 256
* neverland: looks good with dark background and t_Co set to 256
* bubblegum: looks pretty with light background and t_Co set to 256
* mustang: looks good with light background and t_Co set to 256
* mustang: looks good with dark background and t_Co set to 256
* lilypink: looks pretty with light background and t_Co set to 256
* xoria256: looks so-so with light background and t_Co set to 256

Take note that in this role, by default, the t_Co Vim variable is set to 256 in
the template that creates the Vim's main configuration file

Known bugs
----------

* [Emmet plugin](https://github.com/mattn/emmet-vim) is not working due to an
unknown reason

Acknowledgments
---------------

I'm reusing the excellent work of other people who has selected a very useful
set of Vim plugins. The main references are:

- [Examples of Vundle](https://github.com/VundleVim/Vundle.vim/wiki/Examples)
- [mutewinter's Vim Config of Champions](https://github.com/mutewinter/dot_vim)
- [Vim configuration of Juan Pedro Fisanotti](https://github.com/fisadev/fisa-vim-config)
- [Vim settings of axiaoxin](https://github.com/axiaoxin/vim-settings)
- Many useful plugins and configurations for Vim freely available

Thanks to all of them!

Support and enhancements
------------------------

Please let me know your suggestions about how to improve this Ansible role
and I would try to review, analyze and possibly include them.

License
-------

You can use this software for your own purposes, as long as you follow and
accept all of the terms of any of these licenses:

* [MIT license](www.opensource.org/licenses/mit-license.php)
* [Apache License 2.0](www.apache.org/licenses/LICENSE-2.0)
