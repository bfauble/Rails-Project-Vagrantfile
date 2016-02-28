require 'yaml'

config_data = YAML.load_file('vagrant_config_data.yml')
VAGRANTFILE_API_VERSION = '2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.berkshelf.enabled = true
  config.vm.box = 'ubuntu/trusty64'

  config.vm.network :forwarded_port, host: 1080, guest: 1080 # MailCatcher
  config.vm.network :forwarded_port, host: 11211, guest: 11211 #Memcached
  config.vm.network :forwarded_port, host: 5432, guest: 5432 #Postgres
  config.vm.network :forwarded_port, guest: 3000, host: 3000 # Rails
  config.vm.network :forwarded_port, host: 6379, guest: 6379 #Redis

  config.ssh.forward_agent = true

  config.vm.provider 'virtualbox' do |vb|
    # Customize the amount of memory on the VM:
    vb.memory = '2048'
  end

  config.vm.provision :chef_solo do |chef|
    chef.add_recipe 'build-essential'
    chef.add_recipe 'heroku-toolbelt'
    chef.add_recipe 'memcached'
    chef.add_recipe 'nodejs'
    chef.add_recipe 'postgis'
    chef.add_recipe 'redisio'
    chef.add_recipe 'redisio::enable'
    chef.add_recipe 'ruby_build'
    chef.add_recipe 'ruby_rbenv::user'
    chef.add_recipe 'simple-mailcatcher'

    chef.json = {
      nodejs: {
        install_method: 'binary',
        binary: {
          checksum: {
            linux_x64: '6b10e446b5a1227673b87d840e9a500f5d2dbd2b806d96e2d81d634c3381a5f1'
          }
        },
        version: '5.6.0',
        npm: {
          version: '3.8.0'
        }
      },
      postgresql: {
        config: {
          listen_addresses: '*',
          port: '5432'
        },
        pg_hba: [
          {
            type: 'local',
            db: 'all',
            user: 'postgres',
            addr: nil,
            method: 'trust'
          },
          {
            type: 'host',
            db: 'all',
            user: 'all',
            addr: 'samenet',
            method: 'trust'
          }
        ],
        password: {
          postgres: 'password'
        }
      },
      ruby_build: {
        git_ref: 'v20160228'
      },
      rbenv: {
        git_url: 'git://github.com/rbenv/rbenv.git',
        git_ref: 'v1.0.0',
        upgrade: true,
        user_installs: [
          {
            user: 'vagrant',
            rubies: ['2.2.4'],
            global: '2.2.4',
            gems: {
              '2.2.4' => [
                { name: 'bundler', version: '~> 1.11' },
                { name: 'rake', version: '~> 10.5' },
                { name: 'rails', version: '~> 4.2' }
              ]
            }
          }
        ]
      },
      redisio: {
        servers: [{ port: '6379', address: '0.0.0.0'}]
      },
      vagrant: {
        system_chef_solo: '/opt/ruby/bin/chef-solo'
      }
    }

  end
  config.vm.provision 'shell', inline: "echo 'yes' | sudo apt-get autoremove"
  config.vm.provision 'shell', inline: "echo 'cd /vagrant' >> /home/vagrant/.bashrc", privileged: false
  config.vm.provision 'shell', inline: "git config --global user.email '#{config_data['EMAIL']}'", privileged: false
  config.vm.provision 'shell', inline: "git config --global user.name '#{config_data['FULL_NAME']}'", privileged: false
  config.vm.provision 'shell',
    name: 'install expect',
    inline: "echo 'yes' | sudo apt-get install expect",
    privileged: false
  config.vm.provision 'shell',
    name: 'heroku login',
    inline: "expect /vagrant/heroku_login.exp '#{config_data['EMAIL']}' '#{config_data['HEROKU_PASSWORD']}'",
    privileged: false
end
