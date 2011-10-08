---
layout: post
title: "Rubygems: 使用 Carrierwave 處理檔案上傳"
date: 2011-10-05 21:47
comments: true
categories: [Rails, Rubygems, Upload]
author: marsz
---

參考來源
-------

* Github [https://github.com/jnicklas/carrierwave](https://github.com/jnicklas/carrierwave)

<!-- more -->

安裝
-------

```ruby Gemfile
    gem 'carrierwave'
```

建立 uploader
--------

    rails generate uploader user_avatar

檔案產生於

    app/uploader/user_avatar_uploader.rb

簡易範例
--------

#### 直接使用

```ruby    
    uploader = UserAvatarUploader.new
    uploader.store!(my_file)
```


#### 掛在 model 裡使用

在 model 裡
```ruby app/models/user.rb
    class User
      mount_uploader :picture, UserAvatarUploader
    end
```

新增 migration, 因欄位名稱是 picture, 所以新增一個 picture 的 string 欄位在 users table 裡

    rails g migration add_column_picture_to_users

```ruby db/migrate/201101011213_add_column_picture_to_users.rb
    add_column :users, :picture, :string
```

```html _form.html.erb
    <input type="file" name="picture" />
```

```ruby users_controller.rb
    u = User.new
    u.picture = params[:picture]
    u.save!
    u.picture.url # => /url/to/file.png
```

在 _form.html.erb 送出 post 後, 便會按照在 user_avatar_uploader.rb 中的設定進行儲存並且回傳檔案網址

uploader 設定
------

#### 儲存

存成檔案, fog  為將檔案上傳至 cdn 用, 稍後會介紹 
```ruby app/uploader/user_avatar_uploader.rb
    storage :file
    # storage :fog
```
可覆寫 store_dir, 以指定儲存路徑, 以 public/ 為基礎
```ruby app/uploader/user_avatar_uploader.rb
    def store_dir
       "uploader/user_avatar"
    end    
``` 
限定檔案附檔名
```ruby app/uploader/user_avatar_uploader.rb
    def extension_white_list
      %w(jpg jpeg gif png)
    end
```
指定檔名, 包含附檔名 (model.id 稍後介紹)
```ruby app/uploader/user_avatar_uploader.rb
    def filename
      # @filename
      "#{model.id}.png"
    end
```
指定預設的 url (e.g.沒有圖的時候)
```ruby app/uploader/user_avatar_uploader.rb
    def default_url
      "/images/fallback/" + [version_name, "default.png"].compact.join('_')
    end
```
#### 使用 Imagemagick 壓縮

Gemfile
```ruby Gemfile
    gem 'rmagick'
```
uploader 加上
```ruby app/uploader/user_avatar_uploader.rb
    include CarrierWave::RMagick
```
在 user_avatar_uploader.rb 中可以自行定義要用哪些 Imagemagick 的 method 處理圖片
```ruby app/uploader/user_avatar_uploader.rb
    # 轉換成 png
    process :convert => 'png'
    # 按比例縮成指定大小並且補白
    process :resize_and_pad => [160, 160]
```
所有可用的 api 見 [https://github.com/jnicklas/carrierwave/blob/master/lib/carrierwave/processing/rmagick.rb](https://github.com/jnicklas/carrierwave/blob/master/lib/carrierwave/processing/rmagick.rb)

若希望可以另做縮圖, 可以透過 version 同時建立與原圖不同的版本
```ruby app/uploader/user_avatar_uploader.rb
    # 版本名稱為 thumb
    version :thumb do
       process :resize_and_pad => [100, 100]
       process :convert => 'png'
    end
    # 版本名稱為 small
    version :small do
       process :resize_and_pad => [160, 160]
       process :convert => 'png'
    end
```

mixin 類似 CarrierWave::RMagick 的 module 可以將更多 RMagick 的 api 應用在 process 中

RMagick api 可參考 

>* doc: [http://studio.imagemagick.org/RMagick/doc/](http://studio.imagemagick.org/RMagick/doc/)
>* source: [https://github.com/rmagick/rmagick/blob/master/lib/RMagick.rb](https://github.com/rmagick/rmagick/blob/master/lib/RMagick.rb)

取得 version :
```ruby
    u = User.find(1)
    u.picture.url # 原圖 url 
    u.picture.thumb.url # thumb 版本的 url
    u.picture.small.url # small 版本的 url
```

#### 上傳至 CDN (以 Amazon S3 為例)

```ruby Gemfile
    gem 'fog'
```
在 storage 改為 fog
```ruby app/uploader/user_avatar_uploader.rb
    storage :fog
```
新增 config/initializer/carrierwave.rb, 內容如下
```ruby config/initializer/carrierwave.rb
    CarrierWave.configure do |config|
      config.fog_credentials = {
        :provider               => 'AWS',       # required
        :aws_access_key_id      => 'XXXXX',       # your aws access key id
        :aws_secret_access_key  => 'xxxxxxxxxx',       # your aws secret access key
        :region                 => 'ap-southeast-1'  # your bucket's region in S3, defaults to 'us-east-1'
      }
      # your S3 bucket name
      config.fog_directory  = 'my_bucket'
      # custome your domain on aws S3, defaults to nil
      config.fog_host       = 'http://myapp.com'
      config.fog_public     = true                                   # optional, defaults to true
      config.fog_attributes = {'Cache-Control'=>'max-age=315576000'}  # optional, defaults to {}
    end
```
依上述範例, 要在 S3 開一個 public bucket 名為 "my_bucket", 地區為新加坡

access key 可至 [https://aws-portal.amazon.com/gp/aws/developer/account/index.html?action=access-key](https://aws-portal.amazon.com/gp/aws/developer/account/index.html?action=access-key) 搜尋

bucket 中儲存的路徑可在 user_avatar_uploader.rb 中的 store_dir 定義
```ruby app/uploader/user_avatar_uploader.rb
    def store_dir
        "user_avatar/#{model.id}"
    end
```

#### uploader 的 methods

當你將 uploader mount 進 model, 就可以在 uploader 中直接取得該 model 的 instance
```ruby app/uploader/user_avatar_uploader.rb
    def filename
       "#{model.id}.png"
    end 
    # and mounted_as
    def store_dir
       "uploader/user_avatar/#{mounted_as}"
    end
```
uploader 有哪些 medthods 可參考 [https://github.com/jnicklas/carrierwave/tree/master/lib/carrierwave/uploader](https://github.com/jnicklas/carrierwave/tree/master/lib/carrierwave/uploader) 

#### 處理 local file 或 remote file

remote file, 參考 [http://stackoverflow.com/questions/5007575/how-to-assign-a-remote-file-to-carrierwave](http://stackoverflow.com/questions/5007575/how-to-assign-a-remote-file-to-carrierwave)
```ruby
    u = User.find(1)
    url = "http://www.google.com/logo.png"
    u.remote_picture_url = url
    u.save
```
local file
```ruby
    u =  User.find(1)
    file_path = "#{Rails.root}/public/images/exmaples/foo.png"
    u.picture = File.open(file_path)
```

