---
author: vipulnsward
categories: お知らせ
date: 2023-06-30
layout: post
published: true
title: CVE、1週間に2回の新規Railsリリース、config.autoload_libなど盛り沢山！
---

皆さん金曜日をいかがお過ごしでしょうか。
この私[Vipul](https://www.saeloun.com/team/vipul)からRailsのコードベースにあった最新の変更をお届けします。

[[CVE-2023-28362] 利用者により与えられたredirect_toへの値によるXSSの可能性](https://discuss.rubyonrails.org/t/cve-2023-28362-possible-xss-via-user-supplied-values-to-redirect-to/83132)  
お済みでなければご自身のRailsアプリケーションを最新版に更新してください！
利用者により与えられた値を使う場合の`redirect_to`にXSS脆弱性の可能性があった件について、セキュリティ修正を当てるRailsバージョン**7.0.5.1**、**6.1.7.4**がリリースされました。
修正の対象はRailsの`redirect_to`メソッドで、HTTPヘッダーの値として非合法な文字を含む値を与えることができていました。
この脆弱性はCVE識別子**_CVE-2023-28362_**に割り当てられました。

[Rails 7.0.6がリリースされました！]({% post_url 2023-06-29-Rails-7-0-6-has-been-released %})  
Rails **7.0.6** がリリースされました。
本リリースには、**7.0.4**リリース以降の直近数箇月にバックポートされた多くのバグ修正が含まれています。

[config.autoload_libを導入](https://github.com/rails/rails/pull/48572)  
新メソッド`config.autoload_lib(ignore:)`により、`lib`フォルダから簡単に自動読み込みすることができるようになりました。

 ```ruby
 # config/application.rb
 config.autoload_lib(ignore: %w(assets tasks))
 ```

大抵、`lib`ディレクトリには自動読み込みまたは積極読み込みされるべきではない副ディレクトリがあります。
この新メソッドではどの副ディレクトリを自動読み込みするかを必要に応じて指定できます。

この新機能についての詳しいことは[自動読み込みの手引き](https://guides.rubyonrails.org/v7.1/autoloading_and_reloading_constants.html)をお読み下さい。

[config.autoload_lib_onceを導入](https://github.com/rails/rails/pull/48610)

メソッド`config.autoload_lib_once(ignore:)`は上で導入した`config.autoload_lib`と似ていますが、libを`config.autoload_once_paths`に追加する点だけが異なります。

`config.autoload_lib_once`を呼び出すことで、アプリケーションの初期化の部分であったとしても`lib`中のクラスとモジュールが自動読み込みされるようになりますが、再読み込みされることはありません。

[不達Eメールをdeliver_nowで送れるようになりました](https://github.com/rails/rails/pull/48446)  
この変更では`bouse_now_with`を`ActionMailbox`に追加しています。
不達Eメールをメーラーキューを通すことなく直ちに送りたい場合に便利です。

```ruby
 # 不達Eメールをキューに追加
MyMailbox.bounce_with MyMailer.my_method(args)

# Eメールを直ちに配送
MyMailbox.bounce_now_with MyMailer.my_method(args)
```

[railties:install:migrations用のDATABASEオプション](https://github.com/rails/rails/pull/48579)  
この変更では`DATABASE`新規オプションを`railties:install:migrations`タスクに追加します。

これによりエンジンで`rails
railties:install:migrations`を走らせる際にどのデータベースマイグレーションが複製されるべきかを指定できるようになりました。

```bash
$ rails railties:install:migrations DATABASE=animals
```

[以前非決定的に暗号化されたデータを復号するActive Recordの暗号の対応](https://github.com/rails/rails/pull/48530)  
この変更では`SHA1`ハッシュダイジェストで非決定的に暗号化されたデータを復号する対応が追加されました。

SHA1ハッシュダイジェストで非決定的に暗号化されたデータの復号に対応するActive Recordの新規オプションを追加しています。

```
Rails.application.config.active_record.encryption.support_sha1_for_non_deterministic_encryption = true
```

大域的なダイジェストクラスの代わりにSHA-1が使われている場合に7.0から7.1に更新する際の問題が対処されています。

[ActiveSupport::Deprecationに:report動作を追加](https://github.com/rails/rails/pull/48578)  
この変更では`ActiveSupport::Deprecation`に`:report`動作を追加します。

`config.active_support.deprecation =
:report`はエラー報告器を使って非推奨の警告を`ActiveSupport::ErrorReporter`に報告するよう設定します。

これは実運用で発生した非推奨の報告について、ただログに流れたままにする代わりに、不具合管理表に報告させるようにするのに便利です。


_変更の全一覧については[こちらをご覧下さい](https://github.com/rails/rails/compare/@%7B2023-06-24%7D...main@%7B2023-06-30%7D)。_
_この1週間に[25名の方々](https://contributors.rubyonrails.org/contributors/in-time-window/20230624-20230630)からRailsのコードベースに貢献していただきました！_

良い金曜日をお過ごし下さい！また次回をお楽しみに :-)   

_[購読](https://world.hey.com/this.week.in.rails)するとこうした更新をメールで受け取れます。_
