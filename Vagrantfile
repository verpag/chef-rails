Vagrant::Config.run do |config|
  config.vm.box = "ubuntu-16.04"
  config.vm.forward_port 80, 8000
  config.vm.provision "chef_solo" do |chef|
    chef.json = {
      "run_list": [
          "recipe[apt]",
          "recipe[sudo]",
          "recipe[build-essential]",
          "recipe[ohai]",
          "recipe[runit]",
          "recipe[git]",
          "recipe[nginx]",
          "recipe[nginx::apps]",
          "recipe[redis::server]",
          "recipe[monit]",
          "recipe[monit::ssh]",
          "recipe[rvm::user]",
          "recipe[chef-rails]"
      ],
      "automatic": {
          "ipaddress": "<host_ip>"
      },
      "authorization": {
          "sudo": {
              "groups": ["ubuntu"],
              "users": ["ubuntu"],
              "passwordless": true
          }
      },
      "nginx": {
          "user": "ubuntu",
          "distribution": "xenial",
          "components": ["main"],
          "worker_rlimit_nofile": 30000
      },
      "rvm": {
          "user_installs": [{
              "user": "ubuntu",
              "default_ruby": "2.3.1"
          }]
      },
      "monit": {
          "poll_period": "30",
          "poll_start_delay": "60"
      },
      "chef-rails": {
          "packages": ["imagemagick", "nodejs-dev"]
      }
  }
  end
end
