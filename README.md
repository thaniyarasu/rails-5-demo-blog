how to create a new rails 5 application and deploy it into heroku.
install ruby and rails following by their dependencies.

user@ubuntu-15.04:~$ ruby -v
ruby 2.2.0p0 (2015-01-25 revision 49005) [x86_64-linux]
user@ubuntu-15.04:~$ rails -v
Rails 4.2.0

install edge rails
user@ubuntu-15.04:~$ git clone git@github.com:rails/rails.git ~/packages/edge/rails
user@ubuntu-15.04:~$ export EDGE=/home/user/packages/edge/rails/railties/bin
user@ubuntu-15.04:~$ $EDGE/rails -v
Rails 5.0.0.alpha
user@ubuntu-15.04:~$ $EDGE/rails new --edge --database=postgresql rails-5-demo-blog
user@ubuntu-15.04:~$ cd rails-5-demo-blog
user@ubuntu-15.04:~/rails-5-demo-blog$ git init; git add . ; git commit -m "first commit"
user@ubuntu-15.04:~/rails-5-demo-blog$ $EDGE/rails g --help
user@ubuntu-15.04:~/rails-5-demo-blog$ $EDGE/rails g scaffold Post name desc:text at:timestamp
user@ubuntu-15.04:~/rails-5-demo-blog$ rake db:create; rake db:migrate;
user@ubuntu-15.04:~/rails-5-demo-blog$ git add . ; git commit -m "Post Scaffold Added"
user@ubuntu-15.04:~/rails-5-demo-blog$ $EDGE/rails s

now ,you can browse at localhost:3000

want to run in production

comment this last 2 line in config/database.yml

# username: rails-5-demo-blog
# password: <%= ENV['RAILS_5_DEMO_BLOG_DATABASE_PASSWORD'] %>

add production secret_key into config/secret.yml at last line
<pre>
production:
  secret_key_base: blahblahblah
</pre>
add <pre>root 'posts#index'</pre> into config/routes.rb
add </pre>public/assets/</pre> into .gitignore

user@ubuntu-15.04:~/rails-5-demo-blog$ rake db:create RAILS_ENV=production
user@ubuntu-15.04:~/rails-5-demo-blog$ rake db:migrate RAILS_ENV=production
user@ubuntu-15.04:~/rails-5-demo-blog$ rake assets:precompile
user@ubuntu-15.04:~/rails-5-demo-blog$ RAILS_SERVE_STATIC_FILES=true $EDGE/rails server -e production

now, you can browse your production app at localhost:3000

deploy to heroku

add <pre>ruby '2.2.0'</pre> at top of Gemfile
add <pre>gem 'rails_12factor', group: :production</pre> into Gemfile

user@ubuntu-15.04:~/rails-5-demo-blog$ bundle
user@ubuntu-15.04:~/rails5blog$ git add .;git commit -m "Heroku dependencies Added"


install heroku toolbelt

user@ubuntu-15.04:~/rails-5-demo-blog$ heroku login
user@ubuntu-15.04:~/rails-5-demo-blog$ heroku create
user@ubuntu-15.04:~/rails-5-demo-blog$ git push heroku master
user@ubuntu-15.04:~/rails-5-demo-blog$ heroku run:detached rake db:migrate
user@ubuntu-15.04:~/rails-5-demo-blog$ heroku open

A demo app created at https://github.com/thaniyarasu/rails-5-demo-blog
this demo app deployed into heroku at https://rails-5-demo-blog.herokuapp.com/
