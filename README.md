# new_rails_instructions
A set of instructions for making a published Rails app on Heroku.

Defaults for new rails projects
 
rails new myapp --database=postgresql --webpack=react
 
add a Procfile
 
 
heroku run rake db:migrate
 
heroku local 
 
 
# Basic rails installation
ruby -v 2.5.1
gem install rails -v 5.2.0
rails new earthquakes --database=postgresql --webpack=react
gem install bundler
bundle install
 
# Setting up RVM gems for the project
cd earthquakes
$ rvm 2.5.1
$ rvm gemset create earthquakes
$ touch .rvmrc
add the following into .rvmrc .....
#!/usr/bin/env bash
 
ruby_string="ruby-2.5.1"
gemset_name="earthquakes"
 
rvm use "${ruby_string}@${gemset_name}"
.......
 
# Configs to set up heroku running locally
touch Procfile
add the following into Procfile .....
web: bundle exec rails server -p $PORT
.....
 
# Setup Git
1. Create a remote git repository first without a README
2. Follow instructions to push existing repo into git
 
# File for environment variables locally and deployed
touch .env
add .env to .gitignore
to add environment variables to heroku
heroku config:set key=value
This is the only environment variable you will need in .env file. The master key can be found in master.key file.
RAILS_MASTER_KEY=<entermasterkey>
 
https://medium.com/cedarcode/rails-5-2-credentials-9b3324851336
https://www.engineyard.com/blog/rails-encrypted-credentials-on-rails-5.2
 
The remaining env variables need to go into credentials.yml.enc like so
production:
  aws:
    access_key_id: 123
    secret_access_key: 345
You can access the credentials using Rails.application.credentials.production[:aws][:access_key_id] and Rails.application.credentials.production[:aws][:secret_access_key]
 
EDITOR=vim rails credentials:edit
bin/rails credentials:show
 
 
 
# Run app locally to test if its working
bundle install
rake db:create
 
# Create heroku instance
heroku create
git config --list | grep heroku (make sure this returns stuff)
git push heroku master
heroku run rake db:create
heroku run rake db:migrate
 
# Running server and console locally
heroku local:run rails console
heroku local:run rails server
 
# Running Production console
heroku run rails console
 
 
# IF /lib folder info exists then add this to /config/application.rb
    config.autoload_paths += %W(#{config.root}/lib)
    config.eager_load_paths += %W( #{config.root}/lib )
 
 
 
rails g model State name:string abbreviation:string
 
rails g model Earthquake magnitude:decimal origin_time:datetime latitude:decimal longitude:decimal place:string state:references
 
rails g model ApiEvent type:string last_called_on:datetime
 
 
# Securing the APP
Make sure to setup cookie transfer over ssl only.
https://blog.bigbinary.com/2016/08/24/rails-5-adds-more-control-to-fine-tuning-ssl-usage.html
