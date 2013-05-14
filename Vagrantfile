Vagrant.configure('2') do |config|
  config.vm.box = 'omnibus'
  config.vm.box_url = 'https://s3.amazonaws.com/gsc-vagrant-boxes/ubuntu-12.04-omnibus-chef.box'
  config.ssh.forward_agent = true
  config.vm.synced_folder '~/projects', '/projects'
  
  config.vm.network :forwarded_port, guest: 3000, host: 43000 # rails
  config.vm.network :forwarded_port, guest: 4567, host: 44567 # sinatra
  config.vm.network :forwarded_port, guest: 5432, host: 45432 # postgres
  config.vm.network :forwarded_port, guest: 9292, host: 49292 # rack
  
  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ['cookbooks']
    
    chef.add_recipe 'apt'
    chef.add_recipe 'git'
    chef.add_recipe 'postgresql::server'
    chef.add_recipe 'postgresql::contrib'
    chef.add_recipe 'postgresql::server_dev'
    
    chef.add_recipe 'ruby_build'
    chef.add_recipe 'rbenv::user'
    chef.add_recipe 'redis::install_from_package'
    chef.add_recipe 'oh_my_zsh'
    chef.add_recipe 'locale'
    
    chef.json = {
      postgresql: {
        version: '9.2',
        users: [
          {
            username:   'vagrant',
            password:   'password',
            superuser:  true,
            createdb:   true,
            login:      true
          }
        ]
      },
      rbenv: {
        user_installs: [
          {
            user:   'vagrant',
            rubies: ['1.9.3-p392'],
            global: '1.9.3-p392',
            gems: {
              '1.9.3-p392' => [
                { name: 'bundler' },
                { name: 'pg' }
              ]
            }
          }
        ]
      },
      oh_my_zsh: {
        users: [{
          login: 'vagrant',
          theme: 'robbyrussell',
          plugins: [
            'gem',
            'git',
            'rails3',
            'redis-cli',
            'ruby',
            'heroku',
            'rake',
            'rbenv',
            'capistrano'
          ]
        }]
      }
    }
  end
end
