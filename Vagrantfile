Vagrant::Config.run do |config|
  # config.vm.box = "millisami-10-04-server-amd64"
  config.vm.box = "millisami-11-04-server-amd64"
  config.vm.network "33.33.33.10"

  config.vm.forward_port "http", 80, 8080
  config.ssh.max_tries = 50
  config.ssh.timeout   = 300
  
  # Share an additional folder to the guest VM. The first argument is
  # an identifier, the second is the path on the guest to mount the
  # folder, and the third is the path on the host to the actual folder.
  # config.vm.share_folder "v-data", "/vagrant_data", "../data"
  
  config.vm.provision :chef_solo do |chef|
   # chef.recipe_url = "http://cloud.github.com/downloads/millisami/chef_repo/cookbooks.tar.gz"
   # chef.cookbooks_path = [:vm, "cookbooks"]
   chef.cookbooks_path = ["~/chef-repo/cookbooks"]
   # chef.roles_path = 'chef/roles'
   # chef.add_role "nerd_factory"
   chef.add_recipe "base"
   chef.add_recipe "rvm::install"
   chef.add_recipe "rvm::ruby_192"
   
   chef.add_recipe "mysql::client"
   chef.add_recipe "mysql::server"
   
   # chef.add_recipe "redis2::default"
   chef.add_recipe "redis2::default_instance"
   
   chef.add_recipe "app"
   chef.add_recipe "app::deploy"
  
   chef.json = { 
       :base => {
         :system_packages => ["tree", "htop", "vim-nox"]
       },
       :bluepill => { :bin => "/usr/local/rvm/gems/ruby-1.9.2-p290/bin/bluepill" },
       :app => {
         :repository => "git://github.com/millisami/NerdFactory.git",
         :rails_env => "production"
       },
       :unicorn => { :bundled => true },
       :db => {
         :adapter => "mysql",
         :name => "nerdfactory_production",
         :host => "localhost"
       },
       :mysql => {
         :server_root_password => "dbpassword"
         }
     }
  end

  # config.vm.provision :chef_client do |chef|
  #   chef.chef_server_url = "https://api.opscode.com/organizations/sprout"
  #   chef.validation_key_path = "/Users/millisami/chef-repo/.chef/sprout-validator.pem"
  #   chef.validation_client_name = "sprout-validator"
  #   
  #    chef.add_recipe "base"
  #    chef.add_recipe "rvm::install"
  #    chef.add_recipe "rvm::ruby_192"
  
     # chef.add_recipe "mysql::client"
     # chef.add_recipe "mysql::server"
     # # chef.add_recipe "rails"
     #   
     # chef.add_recipe "app"
     #   
     # chef.json = { 
     #     :base => {
     #       :system_packages => ["tree", "htop", "vim-nox"]
     #     },
     #     :app => {
     #       :repository => "https://github.com/millisami/NerdFactory.git"
     #     },
     #     :db => {
     #       :adapter => "mysql"
     #     },
     #     :mysql => {:server_root_password => "dbpassword"}
     #   }    
  # end
end
