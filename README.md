# Rails project vagrant box

## Vagrant File includes:

- [Ruby v2.2.4](http://ruby-doc.org/core-2.2.4/)
  - [Rails v4.2](https://rubygems.org/gems/rails/versions/4.2.5.1)
  - [Rake v10.5](https://rubygems.org/gems/rake/versions/10.5.0)
  - [Bundler v1.11](https://rubygems.org/gems/bundler/versions/1.11.2)
- [Postgres 9.3](http://www.postgresql.org/docs/9.3/static/release-9-3.html)
  - [Postgis](http://postgis.net/)
- [NodeJS v5.6.0](https://nodejs.org/docs/v0.5.6/)
- [NPM v3.8.0](https://www.npmjs.com/)
- [Rbenv v1.0.0](https://github.com/rbenv/rbenv)
- [Ruby Build](https://github.com/rbenv/ruby-build)
- [Redis](http://redis.io/documentation)
- [Memcached](https://github.com/memcached/memcached)
- [Heroku toolbelt](https://toolbelt.heroku.com/)
- [Mailcatcher](http://mailcatcher.me/)

## Install Chef Development Kit
https://downloads.chef.io/chef-dk/mac/

## Install Vagrant:
https://www.vagrantup.com/downloads.html

## Install Berkshelf Plugin:
``$ vagrant plugin install vagrant-berkshelf``

## vagrant_config_data.yml file
```
EMAIL:
FULL_NAME:
HEROKU_PASSWORD:
```

Used to configure git name and email in global config and login to heroku during vagrant up provisioning.

## Put Vagrantfile, Berksfile, heroku_login and vagrant_config_data into project folder and run
``$ vagrant up``
