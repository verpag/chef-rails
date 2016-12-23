Vagrant::Config.run do |config|
  config.vm.box = "geerlingguy/ubuntu1604"
  config.vm.forward_port 80, 8000
  config.vm.provision "chef_solo" do |chef|
    chef.cookbooks_path = ["site-cookbooks", "cookbooks"]
    chef.json = {
      "run_list": [
          "recipe[apt]",
          "recipe[sudo]",
          "recipe[build-essential]",
          "recipe[ohai]",
          "recipe[runit]",
          "recipe[git]",
          "recipe[postgresql]",
          "recipe[postgresql::contrib]",
          "recipe[postgresql::server]",
          "recipe[nginx]",
          "recipe[nginx::apps]",
          "recipe[redis::install_from_package]",
          "recipe[redis::client]",
          "recipe[monit]",
          "recipe[monit::ssh]",
          "recipe[monit::nginx]",
          "recipe[monit::postgresql]",
          "recipe[monit::redis-server]",
          "recipe[rvm::user]",
          "recipe[chef-rails]"
      ],
      "automatic": {
          "ipaddress": "127.0.0.1"
      },
      "authorization": {
          "sudo": {
              "groups": ["ubuntu"],
              "users": ["vagrant"],
              "passwordless": true
          }
      },
      "postgresql": {
        "contrib": {
          "extensions": ["pg_stat_statements"]
        },
        "config": {
         "shared_buffers": "125MB",
         "shared_preload_libraries": "pg_stat_statements"
          },
        "password": {
          "postgres": "psql_passwd"
        }
      },
      "nginx": {
          "users": ["vagrant"],
          "distribution": "xenial",
          "components": ["main"],
          "worker_rlimit_nofile": 30000,
          "apps": {
            "example.com": {
              "listen": [80],
              "server_name": "example.com www.example.com",
              "public_path": "/home/deploy/production.example.com/current/public",
              "upstreams": [
                {
                  "name": "example.com",
                  "servers": [
                    "unix:/home/deploy/production.example.com/shared/pids/example.com.sock max_fails=3 fail_timeout=1s"
                  ]
                }
              ],
              "locations": [
                {
                  "path": "/",
                  "directives": [
                    "proxy_set_header X-Forwarded-Proto $scheme;",
                    "proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;",
                    "proxy_set_header X-Real-IP $remote_addr;",
                    "proxy_set_header Host $host;",
                    "proxy_redirect off;",
                    "proxy_http_version 1.1;",
                    "proxy_set_header Connection '';",
                    "proxy_pass http://example.com;"
                  ]
                },
                {
                  "path": "~ ^/(assets|fonts|system)/|favicon.ico|robots.txt",
                  "directives": [
                    "gzip_static on;",
                    "expires max;",
                    "add_header Cache-Control public;"
                  ]
                }
              ]
            },
            "example2.com": {
              "listen": [80],
              "server_name": "example2.com www.example2.com",
              "public_path": "/home/deploy/production.example2.com/current/public",
              "upstreams": [
                {
                  "name": "example2.com",
                  "servers": [
                    "0.0.0.0:3000 max_fails=3 fail_timeout=1s",
                    "0.0.0.0:3001 max_fails=3 fail_timeout=1s"
                  ]
                }
              ],
              "locations": [
                {
                  "path": "/",
                  "directives": [
                    "proxy_set_header X-Forwarded-Proto $scheme;",
                    "proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;",
                    "proxy_set_header X-Real-IP $remote_addr;",
                    "proxy_set_header Host $host;",
                    "proxy_redirect off;",
                    "proxy_http_version 1.1;",
                    "proxy_set_header Connection '';",
                    "proxy_pass http://example2.com;"
                  ]
                },
                {
                  "path": "~ ^/(assets|fonts|system)/|favicon.ico|robots.txt",
                  "directives": [
                    "gzip_static on;",
                    "expires max;",
                    "add_header Cache-Control public;"
                  ]
                }
              ]
            }
          }
        },
      "rvm": {
          "user_installs": [{
              "user": "vagrant",
              "default_ruby": "2.3.3"
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
