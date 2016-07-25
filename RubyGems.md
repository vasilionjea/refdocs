# Ruby Gems

- https://rubygems.org
- http://guides.rubygems.org/rubygems-basics

The RubyGems software allows you to download, install, and use ruby software packages on your system. A package 
is called a “gem” and contains a Ruby application or library. RubyGems is preinstalled with your Ruby environment.

## Installing & uninstalling gems

Install a gem

    gem install rails
  
Uninstall a gem

    gem uninstall rails

Check RubyGems version

    gem -v

List local gems whose name begins with 'D'

    gem list D

Update your local version of RubyGems

    gem update --system

Create a gem

  - http://guides.rubygems.org/make-your-own-gem

List all gem commands

    gem help commands

Display information about the RubyGems environment

    gem environment

Show the dependencies of an installed gem

    gem dependency rake
    gem dependency | less # shows dependencies for all gems installed locally for your ruby environment

Documentation and gem repository HTTP server

    gem server
