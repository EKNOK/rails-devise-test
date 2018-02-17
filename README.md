# devise_rails5

# Railsのdeviseを使ってログイン

## version

- rails : 5.1.5
- ruby : 2.4.1
- device : 4.4.1


## 参考URL
Qiita https://qiita.com/cigalecigales/items/f4274088f20832252374  


## メモ



## deviseの設定
まず、devise関連を追加。1~4の設定をする。  

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

# https://localhost:3000/ にアクセスした時表示されるページを指定する。Pagesコントローラとindex,showページを追加し、`routes.rb`に指定する。  


  3. Ensure you have flash messages in app/views/layouts/application.html.erb.
     For example:
 
       <p class="notice"><%= notice %></p>
       <p class="alert"><%= alert %></p>

ログインした際に上にフラッシュメッセージが出るようにする。`app/views/layouts/application.html.erb`に追加。

 
  4. You can copy Devise views (for customization) to your app by running:
 
       rails g devise:views

Dviseの導入で追加されるViewは、このコマンドで実行しないとデザインを変更できないので、実行する。
以下、`app/views/devise/..`で追加されるファイル。
shared/_links.html.erb (リンク用パーシャル)
confirmations/new.html.erb (認証メールの再送信画面)
passwords/edit.html.erb (パスワード変更画面)
passwords/new.html.erb (パスワードを忘れた際、メールを送る画面)
registrations/edit.html.erb (ユーザー情報変更画面)
registrations/new.html.erb (ユーザー登録画面)
sessions/new.html.erb (ログイン画面)
unlocks/new.html.erb (ロック解除メール再送信画面)
mailer/confirmation_instructions.html.erb (メール用アカウント認証文)
mailer/password_change.html.erb （メール用パスワード変更完了文）
mailer/reset_password_instructions.html.erb (メール用パスワードリセット文)
mailer/unlock_instructions.html.erb (メール用ロック解除文)
```

## Userモデルの設定

### Userモデルの作成

`$ rails g device User`  
マイグレーションファイルができる。  
Userモデルはデフォルトで`database_authenticatable`、`regiterable`、`recoverable`、`rememberable`、`trackable`、`validatable`が使える。  
以下各モジュール説明。  

|    機能     |     概要    |
|:----------:|:-----------:|
| database_authenticatable | サインイン時にユーザーの正当化を検証するためにパスワードを暗号化してDBに登録する。認証方法としてはPOSTリクエストかHTTP Basic認証が使える。(`bycrypt`みたい) |
| registerable | 登録処理を通してユーザーをサインアップする。また、ユーザー自身のアカウントを編集したり削除することができる |
| recoverable | パスワードをリセットし、それを通知する。 |
| Rememberable | 保存されたcookieからユーザーを記憶するためのトークンを生成・削除する |
| validatable | Emailやパスワードのバリデーションを提供する。独自に定義したバリデーションを追加することも可能 |
| confirmable | メールに記載されているURLをクリックして本登録を完了する。サインイン中にアカウントが認証済みがどうか検証する。 |
| lockable | 一定回数サインインを失敗するとアカウントをロックする。ロック解除にはメールか一定時間経つと解除されるの方法がある。 |
| timeoutable | 一定時間活動していないアカウントのセッションを破棄する。 |
| omniauthable | `intridea/omniauth`をサポートする。Twitter,Facebookの認証を追加する時に使う。 |

### Userモデルの編集
生成されたモジュールの他にもデフォルトでないものも`models/user.rb`に追加する。今回はTwitter。

```ruby
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable,
         :confirmable, :lockable, :timeoutable, :omniauthable, omniauth_providers: [:twitter]
end
```

### マイグレーションファイルの編集
モデルに設定した使用モジュールに対応する部分のコメントアウトを外す。(上の通りだと今回は全て)
`db/migrate/20161112121754_devise_create_users.rb`  

### omniauth-twitter用のカラム追加
omniauth-twitterで使う`provider`と`uid`、Twitter認証の場合はアカウント名を保存したいので、`username`もUserテーブルに追加する。なお、string型。  
`$ rails g migration add_columns_to_users provider uid username`  

### viewの編集
以下ファイルを編集し、navbarが出るようにする。  
`user_signed_in?`はdeviseのメソッド。
`app/views/application.html` (navbarを設置)  
`app/views/pages/index.html.erb`(トップページ)  
`app/views/pages/show.html.erb` (マイページ)   

  現状ではサインアップを開くと`app/models/user.rb`で`omniauthable`に対して何も設定していないのでエラー。  


## Twitterで認証

### 設定
***
### Twitter Developerの登録
[Twitter developer]("https://apps.twitter.com/")にアクセスしてプロジェクトを作成する。
(Settingsタブの下の方のAllow this application to be used to Sign in with Twitterにチェックが入っていることを確認。)  
ss
