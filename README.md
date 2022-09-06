# Dotfiles

This ansible role can be used to install my
[dotfiles](https://github.com/schuam/.dotfiles) on an ArchLinux or Ubuntu
system. Well, I guess this should work on pretty much any other Debian system
as well, but I haven't checked that.

Whenever possible, I try to organize my configuration files according to the
[XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html).
Which means that for example the configuration files (dotfiles) for program
`XYZ` are placed in the directory `$XDG_CONFIG_HOME/XYZ/`. A lot of programs
follow this convention and expect their configuration files at that location.
Some programs don't follow this convention natively, but make it possible to
use it by letting the user set certain environment variables. In such cases, I
set these variables in my .bash_profile file. And than there are programs where
it's just not possible to place the dotfiles into a directory under
$XDG_CONFIG_HOME. For most of these programs, I don't organize their
configuration in my dotfiles repository, with just one exception `bash` (Why
bash? Why?).

Anyways, for the most part, the structure of my dotfiles repository looks like
this:

```
.dotfiles
└── .config
    ├── PROGRAM_1
    └── PROGRAM_2
```

So "all" this role has to do is: create symlinks in `$XDG_CONFIG_HOME` to the
`PROGRAM_x` directories in the repository. These symlinks just have to have the
same name as the target directories. And that's what this rule does, plus some
extra treatment for `bash`.


## Requirements

Git has to be installed on the system you run the role against. You don't
really have to take care of it beforehand, because there is a pre task that
will make sure Git is indeed installed.


## Role Variables

In `defaults/main.yml` you can find the following variables. They determine the
location of the git repository holding the dotfiles, the version (branch) that
is supposed to be used, and the destination on the local system to where the
git repo will be cloned. The last variables in this list is actually not
needed, since the repo is cloned via https and not SSH, but I just have it in
here for good measure.

If you want to use this role to install my dotfiles on your system, feel free
to to so. If you want it to use your own dotfiles repository, and/or put the
repo somewhere else on your system, just change these variables.

- dotfiles_repo: "https://github.com/schuam/.dotfiles.git"
- dotfiles_repo_version: main
- dotfiles_repo_local_destination: "~/.dotfiles"
- dotfiles_repo_accept_hostkey: false

In `vars/main.yml` you can find a list of bash configuration files. The
reason for the need of that list is explained in the description of the role
(first section of this README).

- bash_configs:
    - .bash_logout
    - .bashrc
    - .bash_profile
    - .bash

Also in `vars/main.yml` some directories are defined that will be created. Some
programs need them otherwise they would put stuff in my home directory which I
don't want. The `directory_types` list is a list of different types of
directories that are supposed to be created. The other variables are diconaries
that each define a `base` directory and a list of `dirs` that are created
under the base directory.

- directory_types
- opt_type_dirs
- xdg_cache_home_type_dirs
- xdg_data_home_type_dirs


## Dependencies

None


## Example Playbook

    - hosts: localhost
      roles:
         - { role: schuam.dotfiles }


## License

MIT


## Author Information

This role was created in 2022 by [Andreas Schuster](https://www.schuam.de/).

