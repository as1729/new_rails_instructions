# Starting a Rails App

## Pre-Requisites
- Git must be setup
- Heroku CLI must be setup
- RVM installed
- Appropriate Ruby versions and installing Rails
`rvm use ruby-2.5.5`
`gem install rails -v 6.0.0-rc2`
`gem install bundler`

## Steps
1. A set of instructions for making a published Rails app on Heroku. Defaults for new rails projects.

2. Create the new app locally

`rails new myapp --database=postgresql --webpack=react`
`bundle install`
`cd myapp`

3. Add a Procfile

`touch Procfile`

Add the following code to the Procfile
```
web: bundle exec rails server -p $PORT
```

4. Setup RVM for Project
Make sure you are in the project directory.
`rvm use ruby-2.5.5`
`rvm gemset create myapp`
`touch .rvmrc`

Add the following into `.rvmrc`

```
#!/usr/bin/env bash

ruby_string="ruby-2.5.1"
gemset_name="earthquakes"

rvm use "${ruby_string}@${gemset_name}"
```

5. Push to Git
- Create a remote git repo without a README.
- Follow their instructions to point your local repo to the remote.

6. Run Rails App locally
a. Regular Rails way
`bundle install`
`rake db:create`
`rails c`
`rails s`

b. Using `heroku:local`
```
heroku local:run rails console
heroku local:run rails server
```

7. Create a Heroku Dyno and Push to Heroku
```
heroku create
git config --list | grep heroku (make sure this returns stuff)
git push heroku master

heroku run rake db:create # This might not work due to "access issues". You can migrate directly.
heroku run rake db:migrate
```
`heroku local`
`heroku open # opens the link to the app online`

8. Access Production Heroku Rails Console
`heroku run rails console`

## File for environment variables locally and deployed
`touch .env`
`add .env to .gitignore`
to add environment variables to heroku
`heroku config:set key=value`
This is the only environment variable you will need in .env file. The master key can be found in `master.key`file.
`RAILS_MASTER_KEY=<entermasterkey>`

https://medium.com/cedarcode/rails-5-2-credentials-9b3324851336
https://www.engineyard.com/blog/rails-encrypted-credentials-on-rails-5.2

The remaining env variables need to go into `credentials.yml.enc` like so
production:
  aws:
    access_key_id: 123
    secret_access_key: 345
You can access the credentials using Rails.application.credentials.production[:aws][:access_key_id] and Rails.application.credentials.production[:aws][:secret_access_key]

`EDITOR=vim rails credentials:edit`
`bin/rails credentials:show`

## IF /lib folder info exists then add this to /config/application.rb
```
    config.autoload_paths += %W(#{config.root}/lib)
    config.eager_load_paths += %W( #{config.root}/lib )
```

## Securing the APP
Make sure to setup cookie transfer over ssl only.
https://blog.bigbinary.com/2016/08/24/rails-5-adds-more-control-to-fine-tuning-ssl-usage.html
