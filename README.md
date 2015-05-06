Dotfiles
=========

A role to manage your dotfiles and install requirements for OSX and Debian/Ubuntu.

I use this to manage my various developer environments. I use OSX on my laptops, and Debian
or Ubuntu on my cloud servers and Vagrant VMs. This role ensures I have a consistent working
environment. It revolves around symlinking your dotfiles to ensure your dotfiles repo stays
up to date. It started as a simple Python script and evolved to an Ansible role after I had
to rebuild my Devstack VMs from scratch repeatedly.

The role was originally designed to be run locally, because of all the symlinking and it was part
of my dotfiles repo. I plan to fix that so I can more easily remotely bootstrap VMs/dev boxes.

To install:  
    
    sudo ansible-galaxy install  ServerCobra.dotfiles
    
To run, create a YAML file called dotfiles.yml and modify the example playbook
below. Then, to run the playbook on your local machine:

    ansible-playbook --ask-sudo-pass -v -i "localhost," -c local -v dotfiles.yml


Role Variables
--------------
    
    # The base directory containing files to be symlinked. This is probably your 
    # dotfiles repo.
    dotfiles_src_dir: ~/src/dotfiles/files
    
    # Directories to create before symlinking
    dotfiles_create_dirs:
      - ~/.ssh/

    # List of files to be symlinked, using dotfiles_src_dir as the base
    dotfiles_link_files:
      - { src: 'bashrc', dest: '~/.bashrc' }
    
    # Files to template instead of symlink (only pip.conf is included right now)
    dotfiles_template_files:
      - { src: 'pip.conf', dest: '~/.pip/pip.conf' }
    
    # Apt repos to add before install Apt packages on Debian/Ubuntu
    dotfiles_apt_ppas:
      - docker-maint/testing
    
    # Apt packages to install on Debian/Ubuntu systems
    dotfiles_debian_packages:
      - python-pip
    
    # Homebrew repos to tap before running homebrew installs on OSX
    dotfiles_homebrew_taps:
      - homebrew/dupes
    
    # Homebrew packages to install on OSX
    dotfiles_homebrew_packages:
      - the_silver_searcher
    
    # Homebrew packages to install on OSX with custom options
    dotfiles_homebrew_packages_with_options:
      - { package: 'vim', options: 'override-system-vi' }
    
    # Homebrew packages to install with cask
    dotfiles_cask_packages:
      - iterm2
    
    # Global Python packages to install on OSX and Debian/Ubuntu
    dotfiles_global_python_packages:
      # These 4 are picky about being installed in order..
      - ipython

Example Playbook
----------------

Here's my actual playbook

    - hosts: dev_vm
      roles:
         - role: ServerCobra.dotfiles
           dotfiles_src_dir: ~/src/dotfiles/files
           dotfiles_create_dirs:
             - ~/.ssh/
             - ~/.ipython/profile_default/
             - ~/Library/Preferences/
             - ~/.pip/
           dotfiles_link_files:
             - { src: 'bashrc', dest: '~/.bashrc' }
             - { src: 'bash_aliases', dest: '~/.bash_aliases' }
             - { src: 'bash_exports', dest: '~/.bash_exports' }
             - { src: 'bash_profile', dest: '~/.bash_profile' }
             - { src: 'bash_prompt', dest: '~/.bash_prompt' }
             - { src: 'git_complete.sh', dest: '~/.git_complete.sh' }
             - { src: 'django_complete.sh', dest: '~/.django_complete.sh' }
             - { src: 'ironic_complete.sh', dest: '~/.ironic_complete.sh' }
             - { src: 'gitconfig', dest: '~/.gitconfig' }
             - { src: 'gitignore', dest: '~/.gitignore' }
             - { src: 'supernova', dest: '~/.supernova' }
             - { src: 'agignore', dest: '~/.agignore' }
             - { src: 'functions', dest: '~/.functions' }
             - { src: 'path', dest: '~/.path' }
             - { src: 'nanorc', dest: '~/.nanorc ' }
             - { src: 'ipython_config.py', dest: '~/.ipython/profile_default/ipython_config.py' }
             - { src: 'ssh/config', dest: '~/.ssh/config' }
             - { src: 'ssh/known_hosts', dest: '~/.ssh/known_hosts' }
             - { src: 'PyCharm40', dest: '~/Library/Preferences/PyCharm40' }
             - { src: 'weechat', dest: '~/.weechat' }
           dotfiles_template_files:
           - { src: 'pip.conf', dest: '~/.pip/pip.conf' }
           dotfiles_debian_packages:
             - python-pip
             - python-tox
             - python3.3
             - python3.3-dev
             - python-dev
             - libssl-dev
             - python-pip
             - libmysqlclient-dev
             - libxml2-dev
             - libxslt-dev
             - libpq-dev
             - git
             - git-review
             - libffi-dev
             - vim
             - docker.io
             - silversearcher-ag
             - gettext
             - memcached
             - syslinux
             - s3cmd
             - python-gdbm
             - tmux
             - weechat
           dotfiles_homebrew_taps:
             - homebrew/dupes
             - homebrew/completions
           dotfiles_homebrew_packages:
             - python
             - aspell
             - ssh-copy-id
             - the_silver_searcher
             - tmux
             - pianobar
             - nano
             - mysql- openssl
             - vagrant-completion
             - android-sdk
             - coreutils
             - moreutils
             # Install GNU `find`, `locate`, `updatedb`, and `xargs`, `g`-prefixed.
             - findutils
             # Install Bash 4.
             # Note: don’t forget to add `/usr/local/bin/bash` to `/etc/shells` before
             # running `chsh`.
             - bash
             - bash-completion
             - binutils
             - binwalk
             - fcrackzip
             - nmap
             - pngcheck
             - socat
             - sqlmap
             - tcpflow
             - tcpreplay
             - tcptrace
             - xz
             # Install other useful binaries.
             - ack
             - git
             - lua
             - lynx
             - p7zip
             - pigz
             - pv
             - rename
             - speedtest_cli
             - tree
             - webkit2png
             - imagemagick
             - gnu-sed
             - wget
             - grep
             - screen
             - fleetctl
             # Install Node.js. Note: this installs `npm` too, using the recommended
             # installation method.
             - node
             - postgresql
             - colordiff
           dotfiles_cask_packages:
             - battery-guardian
             - github
             - google-chrome
             - iterm2
             - rescuetime
             - steam
             - vagrant
             - vagrant-manager
             - virtualbox
             - vmware-fusion
             - dropbox
             - boot2docker
             - chromecast
             - firefox
             - utorrent
             - vlc
             - skype
           dotfiles_homebrew_packages_with_options:
             - { package: 'vim', options: 'override-system-vi' }
           dotfiles_global_python_packages:
             - rackspace-auth-openstack
             - keyring
             - rackspace-novaclient
             - supernova
             - pyrax
             - python-gflags
             - python-glanceclient
             - python-ironicclient
             - python-keystoneclient
             - python-neutronclient
             - python-swiftclient
             - requests
             - stevedore
             - awscli
             - ipython
             - plypatch
           dotfiles_apt_ppas:
             - fkrull/deadsnakes
             - nesthib/weechat
             - docker-maint/testing


License
-------

MIT

