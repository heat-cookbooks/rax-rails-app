# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
  end
  config.vm.hostname = 'railsstack'
  config.vm.box = 'ubuntu-12.04'
  config.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_#{config.vm.box}_chef-provisionerless.box"
  config.vm.network "forwarded_port", guest: 80, host: 8383,
    auto_correct: true
  config.omnibus.chef_version = 'latest'
  config.berkshelf.enabled = true
  config.vm.provision "shell",
    inline: "apt-get update"

  config.vm.provision :chef_solo do |chef|
    chef.json = {
      :railsstack => {
        :ruby_manager => 'chruby',
        :app_server => 'unicorn',
        :web_server => 'nginx',
        :ruby_version => '2.1.2',
        # :git_url => 'https://github.com/JasonBoyles/kandan.git',
        :git_url => '',
        :git_revision => 'master',
        :git_deploy_key => 'nil',
        :db => {
          :type => 'postgresql',
          :hostname => 'localhost',
          :name => 'railsappdb',
          :user_id => 'railsapp',
          :user_password => 'superbadpassword'
        },
        :rails => {
          :db_adapter => '',
          # :rake_tasks => ['kandan:bootstrap']
          :rake_tasks => ''
        }
      },
      :mysql => {
          :remove_anonymous_users => true,
          :remove_test_database => true,
          :server_debian_password => 'averydebpassword',
          :server_repl_password => 'averyreplpassword',
          :server_root_password => 'averybadpassword'
        },
      :postgresql => {
        :password => {
          :postgres => 'averybadpassword'
        }
      }
    }

    chef.run_list = [
        'recipe[apt::default]',
        'recipe[build-essential::default]',
        'recipe[rax-rails-app::heat_deploy_app]'
    ]
  end
end
