Devise LDAP Authenticatable - Based on Devise-Imapable
=================

Devise LDAP Authenticatable is a LDAP based authentication strategy for the [Devise](http://github.com/plataformatec/devise) authentication framework.

If you are building applications for use within your organisation which require authentication and you want to use LDAP, this plugin is for you.

Installation
------------

**Please note that Devise LDAP Authenticatable only works with Devise 1.0.6 and Rails 2.3.5 at the moment

Currently this can only be installed as a plugin.

    script/plugin install git@github.com:cschiewek/devise_ldap_authenticatable.git


**And don't forget to add [Devise](http://github.com/plataformatec/devise)!**

either in config/environment.rb:

    config.gem 'devise'

or in bundler

    gem 'devise'


Setup
-----

Once ldap_authenticatable is installed, all you need to do is setup the user model which includes a small addition to the model itself and to the schema.

First the schema :

    create_table :users do |t|

      t.ldap_authenticatable

    end

and indexes (optional) :

    add_index :ldap_login, :unique => true

and don’t forget to migrate :

    rake db:migrate.

then finally the model :

    class User < ActiveRecord::Base

      devise :ldap_authenticatable, :rememberable, :trackable, :timeoutable

      # Setup accessible (or protected) attributes for your model
      attr_accessible :ldap_login, :password, :remember_me

      ...
    end

I recommend using :rememberable, :trackable, :timeoutable as it gives a full feature set for logins.


Usage
-----

Devise LDAP Authenticatable works in replacement of Authenticatable, allowing for LDAP authentication via simple bind. The standard sign\_in routes and views work out of the box as these are just reused from devise. I recommend you run :

    script/generate devise_views

so you can customize your login pages.

------------------------------------------------------------

**please note**

This devise plugin has not been tested with Authenticatable enabled at the same time. This is meant as a drop in replacement for Authenticatable allowing for a semi single sign on approach.


Configuration
----------------------

In initializer  `config/initializers/devise.rb` :

    Devise.setup do |config|
      # ...
      config.ldap_host = 'ldap.mydomain.com'
      config.ldap_port = 389
      # ...
    end

So remember ...
---------------

- don't use Authenticatable

- add ldap\_host and ldap\_port settings in the devise initializer

- generate the devise views and make them pretty


References
----------

* [Devise](http://github.com/plataformatec/devise)
* [Warden](http://github.com/hassox/warden)


TODO
----

LOTS

Released under the MIT license

Copyright (c) 2010 Curtis Schiewek
