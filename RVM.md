
# Ruby Version Manager
- https://rvm.io/
- https://rvm.io/workflow/examples


RVM is a command-line tool, which allows you to easily install, manage, and work with multiple ruby environments.

## Install

Install the latest, _stable_ version

    \curl -sSL https://get.rvm.io | bash -s stable

## Upgrade

Upgrade to the most stable version

    rvm get stable
  
## Install/Uninstall Rubies

Known Rubies (Interpreters)

    rvm list known

Install a ruby (follow the instructions)

    rvm install 2.1.1

Uninstall a ruby

    rvm uninstall 2.1.1

## Choosing current & default ruby

Show your local installed rubies

    rvm list

Ruby version

    ruby -v

Set the default ruby that's used every time you open the terminal

    rvm --default use 2.1.1

Use a specific ruby in this shell session 

    rvm use ruby-1.9.3

Switch back to the selected default ruby

    rvm default

Use the system ruby in this shell session

    rvm use system


