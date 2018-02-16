# devise_rails5

# Railsのdeviseを使ってログイン

## version

- rails : 5.1.5
- ruby : 2.4.1
- device : 4.4.1


## 参考URL
Qiita https://qiita.com/cigalecigales/items/f4274088f20832252374  


## メモ

```console
$ rails g devise:install
Running via Spring preloader in process 2501
      create  config/initializers/devise.rb
      create  config/locales/devise.en.yml
===============================================================================
 
Some setup you must do manually if you haven't yet:
 
  1. Ensure you have defined default url options in your environments files. Here
     is an example of default_url_options appropriate for a development environment
     in config/environments/development.rb:
 
       config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
 
     In production, :host should be set to the actual host of your application.

# `config/enviroment/development.rb`に`config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }`を追加。
 
  2. Ensure you have defined root_url to *something* in your config/routes.rb.
     For example:
 
       root to: "home#index"
 
  3. Ensure you have flash messages in app/views/layouts/application.html.erb.
     For example:
 
       <p class="notice"><%= notice %></p>
       <p class="alert"><%= alert %></p>
 
  4. You can copy Devise views (for customization) to your app by running:
 
       rails g devise:views
```
