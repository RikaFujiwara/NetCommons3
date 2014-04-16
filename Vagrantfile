# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
  config.vm.box = 'precise64'
  config.vm.box_url = 'http://files.vagrantup.com/precise64.box'

  config.vm.network :forwarded_port, guest: 80, host: 9090
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.vm.define 'default' do |node|
    node.vm.hostname = 'app.local'
    node.hostmanager.aliases = %w(sphinx.local phpdoc.local phpldapadmin.local redmine.local jenkins.local)
  end

  # config.vm.synced_folder './', '/vagrant_data', :create => true, :owner=> 'www-data', :group=>'www-data', :mount_options => ['dmode=775','fmode=775']
  # doc_root = '/vagrant_data/app/webroot'
  config.vm.synced_folder '.', '/var/www/app',
    :create => true, :owner=> 'www-data', :group=>'www-data',
    :mount_options => ['dmode=775','fmode=775']
  doc_root = '/var/www/app/app/webroot'

  config.vm.provider :virtualbox do |vb|
    vb.gui = false
    vb.customize ['modifyvm', :id, '--memory', '1024']
  end

  config.berkshelf.enabled = true
  config.omnibus.chef_version = :latest
  json = JSON.parse(Pathname(__FILE__).dirname.join('tools/chef/nodes', 'vagrant.json').read)
  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ['site-cookbooks', 'cookbooks']
    chef.run_list = json.delete('run_list')
    chef.json = json
  end
end
